---
title: "深入理解嵌入式UART驅動程式設計：從函數指標到環形緩衝區"
date: 2025-02-27T06:30:01Z
description: "在嵌入式系統開發中，串列通訊（特別是UART/USART）是與外部設備連接的常用方式。本文將探討如何設計高效的UART驅動程式，重點關注兩個核心技術：函數指標和環形緩衝區。"
canonicalURL: "https://medium.com/@chrislee8613/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%B5%8C%E5%85%A5%E5%BC%8Fuart%E9%A9%85%E5%8B%95%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88-%E5%BE%9E%E5%87%BD%E6%95%B8%E6%8C%87%E6%A8%99%E5%88%B0%E7%92%B0%E5%BD%A2%E7%B7%A9%E8%A1%9D%E5%8D%80-8864a2931d28"
tags: ["嵌入式", "C語言", "UART"]
---
在嵌入式系統開發中，串列通訊（特別是UART/USART）是與外部設備連接的常用方式。本文將探討如何設計高效的UART驅動程式，重點關注兩個核心技術：**函數指標**和**環形緩衝區**。

## 函數指標：實現靈活的回調機制

### 什麼是函數指標？

函數指標是C語言中一個強大特性，允許我們在運行時動態選擇執行的函數。在UART驅動中，它特別適合實現回調（callback）機制。

```c
// 定義函數指標類型
typedef void (*uart_rx_callback_t)(char data);
// 全域函數指標變數
static uart_rx_callback_t g_rx_callback[PORT_NUM] = {NULL};
// 註冊回調函數
void uart_register_callback(uint8_t port, uart_rx_callback_t callback) {
 if (port < PORT_NUM) {
 g_rx_callback[port] = callback;
 }
}
```

### 為什麼使用函數指標？

在UART驅動中，函數指標提供了以下優勢：

1. **解耦合**：硬體驅動層與應用層之間實現松耦合
2. **事件驅動**：實現中斷驅動的事件處理模型
3. **多通道支持**：每個UART埠可配置不同的處理邏輯
4. **靈活性**：應用程式可以在運行時改變處理函數

## 環形緩衝區：高效的數據管理

### 什麼是環形緩衝區？

環形緩衝區（Circular Buffer或Ring Buffer）是一種固定大小的緩衝區，當達到尾部時會自動環繞回開始位置，形成一個邏輯上的環。

```c
typedef struct {
 uint8_t data[UART_BUFFER_SIZE]; // 緩衝區數據
 uint16_t read_index; // 讀指針
 uint16_t write_index; // 寫指針
 bool is_full; // 滿標誌
 bool is_empty; // 空標誌
} uart_ring_buffer_t;
```

### 高效的緩衝區操作

在環形緩衝區實現中，有一個關鍵的優化技巧：使用位元運算代替模運算。

```c
// 寫入一個字節到緩衝區
bool uart_buffer_write(uart_ring_buffer_t *buffer, uint8_t data) {
 if (buffer->is_full) {
 return false; // 緩衝區已滿
 }
 
 // 存儲數據
 buffer->data[buffer->write_index] = data;
 
 // 更新寫指針，關鍵優化在這裡！
 buffer->write_index = (buffer->write_index + 1) & (UART_BUFFER_SIZE - 1);
 
 // 更新緩衝區狀態
 if (buffer->write_index == buffer->read_index) {
 buffer->is_full = true;
 }
 buffer->is_empty = false;
 
 return true;
}
```

### 為什麼UART_BUFFER_SIZE必須是2的冪次方？

注意上面代碼中的這一行：

```c
buffer->write_index = (buffer->write_index + 1) & (UART_BUFFER_SIZE - 1);
```

`x%16`

當 `UART_BUFFER_SIZE`是2的冪次方（如16, 32, 64, 128）時，`(UART_BUFFER_SIZE-1)`的二進位表示全為1位元（如15的二進位是01111）。這使得位元與運算`&`等同於取模運算`%`，但效率高出許多：

- `x%16` 等同於 `x&15`
- `x%32` 等同於 `x&31`
- `x%64` 等同於 `x&63`

> 在嵌入式系統中，位元運算通常比模運算快5–15倍，這對於中斷處理程序中的時間關鍵型操作尤為重要。

## 整合設計：完整的UART驅動實現

現在，讓我們看看如何將函數指標和環形緩衝區結合起來，設計一個完整的UART驅動程式。

```c
// 緩衝區定義
#define UART_BUFFER_SIZE 64 // 必須是2的冪次方！
static uart_ring_buffer_t rx_buffer[PORT_NUM];
static uart_ring_buffer_t tx_buffer[PORT_NUM];
// 回調函數指標
static uart_rx_callback_t rx_callback[PORT_NUM] = {NULL};
static uart_tx_complete_callback_t tx_complete_callback[PORT_NUM] = {NULL};
// 中斷處理函數
void UART1_IRQHandler(void) {
  uint8_t port = 0;
  
  // 接收中斷
  if (UART_GetITStatus(UART1, UART_IT_RXNE)) {
  char rx_data = UART_ReceiveData(UART1);
  
  // 1. 存入環形緩衝區
  uart_buffer_write(&rx_buffer[port], rx_data);
  
  // 2. 調用用戶回調函數
  if (rx_callback[port] != NULL) {
  rx_callback[port](rx_data);
 }
 }
 
 // 發送完成中斷
 if (UART_GetITStatus(UART1, UART_IT_TC)) {
 UART_ClearITPendingBit(UART1, UART_IT_TC);
 
 if (!uart_buffer_is_empty(&tx_buffer[port])) {
 // 還有數據要發送
 uint8_t tx_data;
 uart_buffer_read(&tx_buffer[port], &tx_data);
 UART_SendData(UART1, tx_data);
 } else if (tx_complete_callback[port] != NULL) {
 // 發送完成，調用回調
 tx_complete_callback[port]();
 }
 }
}
```

### 初始化和註冊回調

```c
void uart_init(uint8_t port, uint32_t baudrate) {
 // 硬體初始化代碼省略…
 
 // 初始化緩衝區
 uart_buffer_init(&rx_buffer[port]);
 uart_buffer_init(&tx_buffer[port]);
 
 // 開啟中斷
 UART_ITConfig(UART_INSTANCE[port], UART_IT_RXNE, ENABLE);
}
// 註冊接收回調
void uart_register_rx_callback(uint8_t port, uart_rx_callback_t callback) {
 rx_callback[port] = callback;
}
// 非阻塞式發送函數
bool uart_send_data(uint8_t port, const uint8_t *data, uint16_t length) {
 for (uint16_t i = 0; i < length; i++) {
 if (!uart_buffer_write(&tx_buffer[port], data[i])) {
 return false; // 緩衝區已滿
 }
 }
 
 // 如果發送器空閒，啟動發送
 if (UART_GetFlagStatus(UART_INSTANCE[port], UART_FLAG_TXE)) {
 uint8_t tx_data;
 if (uart_buffer_read(&tx_buffer[port], &tx_data)) {
 UART_SendData(UART_INSTANCE[port], tx_data);
 UART_ITConfig(UART_INSTANCE[port], UART_IT_TC, ENABLE);
 }
 }
 
 return true;
}
```

## 實際應用示例

以下是一個使用這個UART驅動程式的簡單應用示例：

```c
// 自定義接收處理函數
void my_rx_handler(char data) {
 // 回顯接收到的字符
 uart_send_data(0, (uint8_t*)&data, 1);
 
 // 其他處理邏輯…
}
int main(void) {
 // 系統初始化…
 
 // 初始化UART，設置波特率為115200
 uart_init(0, 115200);
 
 // 註冊接收回調
 uart_register_rx_callback(0, my_rx_handler);
 
 // 發送歡迎訊息
 uart_send_data(0, (uint8_t*)"Hello UART!\r\n", 13);
 
 while (1) {
 // 主循環
 // …
 }
}
```

## 性能與優化考慮

在設計UART驅動程式時，有幾個關鍵的性能考慮點：

1. **中斷延遲**：中斷處理程序應該盡可能快速完成。使用環形緩衝區和位元運算可以最小化中斷處理時間。

2. **緩衝區大小**：需要根據應用需求選擇合適的緩衝區大小。太小可能導致溢出，太大會浪費RAM。

3. **防止溢出**：對於高速通訊，應該監測緩衝區狀態，必要時實現流控制。

4. **多UART管理**：使用函數指標陣列可以統一管理多個UART埠，減少代碼冗餘。

## 結論

高效的UART驅動程式設計需要結合多種技術：
- **函數指標**提供靈活的回調機制
- **環形緩衝區**實現高效的數據管理
- **位元運算優化**提升性能
- **中斷驅動模型**實現非阻塞操作

這些技術不僅適用於UART，也可以擴展到其他嵌入式通訊介面，如SPI、I2C等。掌握這些核心概念，將幫助你設計出更高效、可靠的嵌入式驅動程式。

— -

本文討論的技術雖然基於C語言和裸機編程，但其核心理念也適用於RTOS環境或更高階的嵌入式系統開發。希望這些知識能幫助你在實際嵌入式項目中構建更高效的通訊模塊。
