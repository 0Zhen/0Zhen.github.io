---
title: "工程師必備：從JSON到PDF的技術文件自動化工作流程"
date: 2025-04-07T09:57:13Z
description: "在現代軟體開發環境中，技術文件的管理和維護常常被視為一項繁瑣的任務。作為一名開發者，我曾經為撰寫和更新各種專業規格文件（如Error Code Table、SOP、Protocol等）而煩惱。這些文件有著共同的特點：高度重複性和頻繁的版本變化。"
canonicalURL: "https://medium.com/@chrislee8613/%E5%B7%A5%E7%A8%8B%E5%B8%AB%E5%BF%85%E5%82%99-%E5%BE%9Ejson%E5%88%B0pdf%E7%9A%84%E6%8A%80%E8%A1%93%E6%96%87%E4%BB%B6%E8%87%AA%E5%8B%95%E5%8C%96%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B-f3e74c9b5eff"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*NOkUwKcevr-3zzBM-b6mnQ.png"
  relative: false
tags: ["Python", "程式設計", "自動化"]
---
![](https://cdn-images-1.medium.com/max/800/1*NOkUwKcevr-3zzBM-b6mnQ.png)

在現代軟體開發環境中，技術文件的管理和維護常常被視為一項繁瑣的任務。作為一名開發者，我曾經為撰寫和更新各種專業規格文件（如Error Code Table、SOP、Protocol等）而煩惱。這些文件有著共同的特點：高度重複性和頻繁的版本變化。

在本文中，我將分享一套完整的文件自動化解決方案：利用JSON作為資料來源，通過Python處理資料，使用LaTeX生成格式精美的PDF文件，並通過Git實現版本控制和自動發布。

## 為什麼選擇JSON + LaTeX + Git的組合？

在探索各種技術文件解決方案後，我發現這個組合具有以下優勢：

1. **JSON**：結構化數據存儲，易於程式讀取和修改
2. **LaTeX**：專業排版系統，產出高質量PDF文件
3. **Git**：完善的版本控制，支持協作和發布流程

雖然HTML也是一個選項，但對於正式文件來說，PDF格式往往更受青睞，它具有固定的排版和更專業的外觀。

## 工作流程概述

整個自動化工作流程可以概括為以下幾個步驟：

1. 建立JSON文件作為數據源
2. 使用Python處理JSON數據
3. 將處理後的數據轉換為LaTeX格式
4. 使用MiKTeX編譯LaTeX文件生成PDF
5. 利用Git進行版本控制和發布

讓我們深入了解每個步驟的具體實現。

## 步驟一：建立JSON文件

首先，我們需要設計一個適合我們文件需求的JSON結構。以錯誤代碼表（Error Code Table）為例：

```json
{
  "document_info": {
    "title": "系統錯誤代碼表",
    "version": "1.2.0",
    "last_updated": "2025-04-07",
    "author": "工程團隊"
  },
  "error_codes": [
    {
      "code": "E001",
      "severity": "ERROR",
      "message": "連接數據庫失敗",
      "description": "無法建立與數據庫的連接",
      "solution": "檢查數據庫連接字符串和網絡狀態"
    },
    {
      "code": "E002",
      "severity": "WARNING",
      "message": "配置文件不完整",
      "description": "系統配置文件缺少必要參數",
      "solution": "參考文檔補全配置文件中的必要參數"
    }
    // 更多錯誤代碼...
  ]
}
```

這種結構化的格式使得更新和維護變得非常簡單。需要添加新的錯誤代碼？只需在陣列中添加一個新對象即可。

## 步驟二：使用Python處理JSON

接下來，我們編寫Python腳本來處理JSON數據。這一步的目的是：

1. 讀取JSON文件
2. 驗證數據完整性
3. 進行必要的數據轉換
4. 為LaTeX生成做準備

以下是處理JSON的示例Python代碼：

```python
import json
import datetime
import os
def process_error_code_table(json_file_path):
    # 讀取JSON文件
    with open(json_file_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
    
    # 驗證數據完整性
    required_fields = ['document_info', 'error_codes']
    for field in required_fields:
        if field not in data:
            raise ValueError(f"JSON缺少必要欄位: {field}")
    
    # 數據處理和轉換
    # 例如：按照錯誤代碼排序
    data['error_codes'] = sorted(data['error_codes'], key=lambda x: x['code'])
    
    # 更新版本信息
    if 'auto_update_date' in data['document_info'] and data['document_info']['auto_update_date']:
        data['document_info']['last_updated'] = datetime.datetime.now().strftime('%Y-%m-%d')
    
    return data
# 使用函數
error_data = process_error_code_table('error_codes.json')
```

## 步驟三：將JSON轉換為LaTeX

現在是時候將處理好的JSON數據轉換為LaTeX格式了。我們可以使用Python的字符串模板或專門的模板引擎來實現：

```python
def generate_latex(data):
    # LaTeX文檔頭部
    latex_content = r'''\documentclass{article}
    \usepackage[utf8]{inputenc}
    \usepackage{booktabs}
    \usepackage{longtable}
    \usepackage{xcolor}
    \usepackage{hyperref}
    \usepackage{geometry}\geometry{a4paper, margin=1in}
    \title{''' + data['document_info']['title'] + r'''}
    \author{''' + data['document_info']['author'] + r'''}
    \date{版本 ''' + data['document_info']['version'] + r''' - 更新日期: ''' + data['document_info']['last_updated'] + r'''}
    \begin{document}
    \maketitle
    \tableofcontents
    \newpage
    \section{錯誤代碼表}
    \begin{longtable}{|p{0.08\textwidth}|p{0.10\textwidth}|p{0.22\textwidth}|p{0.3\textwidth}|p{0.3\textwidth}|}
    \hline
    \textbf{代碼} & \textbf{嚴重性} & \textbf{錯誤訊息} & \textbf{描述} & \textbf{解決方案} \\
    \hline
    \endhead
    '''
        
        # 添加每個錯誤代碼的行
        for error in data['error_codes']:
            # 根據嚴重性設置顏色
            severity_color = 'red' if error['severity'] == 'ERROR' else 'orange' if error['severity'] == 'WARNING' else 'black'
            
            latex_content += f"{error['code']} & "
            latex_content += f"\\textcolor{{{severity_color}}}{{{error['severity']}}} & "
            latex_content += f"{error['message']} & "
            latex_content += f"{error['description']} & "
            latex_content += f"{error['solution']} \\\\\n\\hline\n"
        
        # 文檔結尾
        latex_content += r'''\end{longtable}
    \end{document}
    '''
        
        return latex_content
    # 生成LaTeX文檔
    latex_content = generate_latex(error_data)
    # 保存LaTeX文件
    with open('error_codes.tex', 'w', encoding='utf-8') as f:
        f.write(latex_content)
    
```

## 步驟四：使用MiKTeX編譯LaTeX生成PDF

現在我們有了LaTeX文件，接下來使用MiKTeX（或其他LaTeX發行版）將其編譯為PDF。我們可以在Python腳本中調用系統命令來完成這一步：

```python
import subprocess

def compile_latex_to_pdf(tex_file_path):
    try:
        # 調用pdflatex兩次以確保目錄和參考正確生成
        subprocess.run(['pdflatex', tex_file_path], check=True)
        subprocess.run(['pdflatex', tex_file_path], check=True)
        
        # 獲取PDF文件名（去掉.tex後綴）
        pdf_file = tex_file_path.replace('.tex', '.pdf')
        
        if os.path.exists(pdf_file):
            print(f"PDF生成成功: {pdf_file}")
            return pdf_file
        else:
            print("PDF生成失敗")
            return None
    except subprocess.CalledProcessError as e:
        print(f"LaTeX編譯錯誤: {e}")
        return None
# 編譯PDF
pdf_file = compile_latex_to_pdf('error_codes.tex')
```

## 步驟五：使用Git進行版本控制和發布

最後，我們將整個流程與Git整合，實現版本控制和自動發布：

```python
def git_publish_document(pdf_file, version):
    try:
        # 添加所有文件到Git
        subprocess.run(['git', 'add', '.'], check=True)
        
        # 提交變更
        commit_message = f"Update documentation to version {version}"
        subprocess.run(['git', 'commit', '-m', commit_message], check=True)
        
        # 添加版本標籤
        tag_name = f"v{version}"
        subprocess.run(['git', 'tag', '-a', tag_name, '-m', f"Version {version}"], check=True)
        
        # 推送到遠端倉庫
        subprocess.run(['git', 'push', 'origin', 'master'], check=True)
        subprocess.run(['git', 'push', 'origin', tag_name], check=True)
        
        print(f"文檔已發布，版本 {version}")
        return True
    except subprocess.CalledProcessError as e:
        print(f"Git操作錯誤: {e}")
        return False
```

```
# 發布文檔
if pdf_file:
    git_publish_document(pdf_file, error_data['document_info']['version'])
```

## 將所有步驟整合

最後，我們可以將上述所有步驟整合到一個完整的Python腳本中，實現一鍵式文檔生成和發布：

```python
import json
import datetime
import os
import subprocess

def main():
    # 1. 處理JSON數據
    error_data = process_error_code_table('error_codes.json')
    
    # 2. 生成LaTeX
    latex_content = generate_latex(error_data)
    with open('error_codes.tex', 'w', encoding='utf-8') as f:
        f.write(latex_content)
    
    # 3. 編譯PDF
    pdf_file = compile_latex_to_pdf('error_codes.tex')
    
    # 4. Git發布
    if pdf_file and input("是否發布文檔? (y/n): ").lower() == 'y':
        git_publish_document(pdf_file, error_data['document_info']['version'])
if __name__ == "__main__":
    main()
```

## 自動化的進階選項

除了基本的文檔生成流程外，我們還可以進一步優化自動化程度：

1. **使用GitHub Actions或Jenkins**：設置CI/CD管道，在JSON變更時自動觸發文檔生成和發布
2. **添加審核流程**：在發布前添加文檔審核步驟
3. **多語言支持**：在JSON中添加多語言數據，生成不同語言版本的文檔
4. **文檔差異比較**：實現對比新舊版本文檔的差異功能

## 結論

通過這套JSON → Python → LaTeX → PDF → Git的自動化工作流程，我們成功地將技術文檔的維護工作簡化為JSON數據的更新。這不僅提高了效率，還確保了文檔的一致性和專業性。

對於那些需要頻繁更新的專業規格文件，如錯誤代碼表、SOP、協議文檔等，這種自動化方法尤為有效。它使團隊可以專注於內容本身，而不是文檔格式和發布過程。

您也可以根據自己的需求調整此工作流程，或者擴展它以支持更多類型的技術文檔。重要的是找到適合您團隊的文檔自動化策略，並堅持使用它。
