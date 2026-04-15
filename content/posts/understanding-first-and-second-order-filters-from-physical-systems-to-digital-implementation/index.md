---
title: "Understanding First and Second-Order Filters: From Physical Systems to Digital Implementation"
date: 2025-02-21T15:35:37Z
description: "As a firmware engineer, I frequently encounter filtering problems in various applications. Whether it’s smoothing sensor data, implementing…"
canonicalURL: "https://medium.com/@chrislee8613/understanding-first-and-second-order-filters-from-physical-systems-to-digital-implementation-81f600d88e23"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*aN2ag3geLYT2T_QgElIToA.jpeg"
  relative: false
tags: ["馬達", "電磁設計", "訊號處理"]
---
![Image by kjpargeter on Freepik](https://cdn-images-1.medium.com/max/800/1*aN2ag3geLYT2T_QgElIToA.jpeg)
*Image by kjpargeter on Freepik*

As a firmware engineer, I frequently encounter filtering problems in various applications. Whether it’s smoothing sensor data, implementing motor controls, or designing PID controllers, these all fundamentally relate to second-order systems. After years of practical experience, I’ve come to realize that understanding the underlying principles of first and second-order filters is crucial for any engineer working with real-world signals.

## Visual Demonstration

Before diving into the theory, let’s look at a practical demonstration of how first and second-order filters behave:

This visualization shows:

- Top: Original signal (blue) with added noise (gray)
- Middle: First-order filter output (red, k=0.100)
- Bottom: Second-order filter output (green, f0=0.500, ζ=0.707)

Notice how each filter type handles the noisy signal differently, with the second-order filter providing more sophisticated control over the response characteristics. Try to change the k, f0 on you own!

## The Evolution of Second-Order Filters

Engineers first noticed that many natural systems exhibited second-order characteristics through studying spring-mass-damper systems. This physical system comprises three essential elements:

- A spring providing restoration force (position-related)
- Mass providing inertia (acceleration-related)
- A damper providing resistance (velocity-related)

As a firmware engineer, I’ve found this physical analogy particularly helpful when tuning PID controllers. The proportional term (P) acts like the spring, the derivative term (D) like the damper, and the integral term (I) accumulates error similar to how mass accumulates momentum.

These observations led to the development of mathematical models described by second-order differential equations:

```bash
m(d²x/dt²) + c(dx/dt) + kx = F
```

where:

- m is mass
- c is the damping coefficient
- k is the spring constant
- F is the external force
- x is displacement

## From First to Second-Order Digital Filters

### First-Order Filter

In my experience, first-order filters are excellent for simple smoothing tasks where minimal delay is crucial. The basic equation is:

```bash
y[n] = y[n-1] + k*(x[n]-y[n-1])
```

or alternatively:

```bash
y[n] = (1-k)*y[n-1] + k*x[n]
```

where k is the filtering coefficient between 0 and 1. I often start with first-order filters for basic sensor noise reduction because they’re intuitive to tune and computationally efficient.

### Second-Order Filter

When I need more control over the response characteristics, especially in motor control applications or when dealing with resonant systems, I turn to second-order filters:

```bash
Y[n] = a1*Y[n-1] + a2*Y[n-2] + b0*X[n] + b1*X[n-1] + b2*X[n-2]
```

This equation corresponds to three physical aspects:

1. Position (signal value)
2. Velocity (first-order rate of change)
3. Acceleration (second-order rate of change)

## Key Parameters and Characteristics

### Critical Parameters

1. Cutoff Frequency (f0): Determines the system’s “elasticity”
2. Damping Coefficient (ζ):

- ζ < 1: Underdamped, exhibits oscillation
- ζ = 1: Critically damped, fastest non-oscillating response
- ζ > 1: Overdamped, slowly approaches target

In my firmware work, I’ve found that starting with critically damped settings (ζ = 1) and then adjusting based on system response is often the most practical approach.

### Coefficient Meanings

```markdown
Feedback Coefficients (a1, a2):

Control system dependency on previous output values
Determine system "memory" capability
Influence resonance characteristics and stability

Feedforward Coefficients (b0, b1, b2):

Control system response to input signals
Determine weights for current and past inputs
Affect sensitivity to input changes
```

## Applications in Firmware Engineering

In my experience, these filters are essential in:

1. Sensor data processing

- Accelerometer/gyroscope data smoothing
- Temperature sensor reading stabilization
- Position encoder noise reduction

2. Motor control

- Speed regulation
- Position control
- Torque smoothing

3. Signal processing

- Audio processing
- Vibration analysis
- Power supply regulation

## Practical Implementation Tips

From my years of firmware development, here are some key insights:

1. Start Simple

- Begin with a first-order filter
- Only move to second-order if you need more control
- Document your tuning process

2. Resource Considerations

- First-order filters are computationally lighter
- Second-order filters need more memory (previous states)
- Consider fixed-point arithmetic for embedded systems

3. Tuning Process

- Start with conservative parameters
- Increase responsiveness gradually
- Always test with real-world signals

## Interactive Comparison Tool

To help engineers better understand these concepts, I’ve created an interactive tool using Streamlit that allows you to:

- Visualize both filter responses in real-time
- Adjust filter parameters and see immediate effects
- Compare filtering performance on various input signals

## Conclusion

After years of working with these filters in firmware development, I’ve come to appreciate their elegant simplicity and power. While the mathematics might seem daunting at first, understanding the physical analogies and having proper visualization tools makes them much more approachable.

Whether you’re working on motor control systems, processing sensor data, or implementing PID controllers, having a solid grasp of first and second-order filters will significantly improve your ability to design robust and efficient systems.
