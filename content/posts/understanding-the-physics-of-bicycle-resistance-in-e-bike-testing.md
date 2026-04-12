---
title: "Understanding the Physics of Bicycle Resistance in E-Bike Testing"
date: 2025-03-24T10:17:10Z
description: "When conducting e-bike tests according to standards such as EN-15194 and R200, accurate calculation of bicycle resistance is crucial…"
canonicalURL: "https://medium.com/@chrislee8613/understanding-the-physics-of-bicycle-resistance-in-e-bike-testing-f16a788479aa"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*kybx2Rt5TAgUq-L46imw_g.png"
  relative: false
---
When conducting e-bike tests according to standards such as EN-15194 and R200, accurate calculation of bicycle resistance is crucial. Different slopes, bicycle weights, and other factors all affect resistance calculations. Let’s explore how to use simple physics principles to calculate these forces more effectively.

## The Physical Basis of Bicycle Resistance

Bicycle resistance is not a single force, but rather a combination of several different forces that a cyclist must overcome. Understanding each component helps engineers design better testing protocols and more efficient e-bikes.

## Calculating Riding Power

When cycling, we encounter air resistance, friction, and gravity. The sum of these three forces constitutes the total resistance. When we know the total resistance, we can calculate the speed and torque needed to climb a slope.

![Create by ChrisLee](https://cdn-images-1.medium.com/max/800/1*kybx2Rt5TAgUq-L46imw_g.png)
*Create by ChrisLee*

```java
Total Resistance = Air Resistance + Rolling Resistance + Climbing Resistance
```

Riding power is the total resistance multiplied by velocity:

```java
Riding Power = Total Resistance × Velocity
```

In practical applications, we typically know the wheel’s rotational speed (RPM) rather than linear velocity. Therefore, we calculate:

```java
Wheel Circumference = Wheel Diameter × π
Equivalent RPM = Velocity / Wheel Circumference
Riding Power = Total Resistance × Wheel Radius × Equivalent RPM × 2π / 60
```

Where 2π/60 is the factor that converts rotations per minute to angular velocity per second. Now, let’s analyze these three forces.

## 1. Air Resistance

Air resistance is the hindrance caused by air when a bicycle is in motion. According to fluid dynamics equations:

```java
Air Resistance = 0.5 × Air Density × Drag Coefficient × Frontal Area × Velocity²
```

Where:

- **Air Density**: Mass of air per unit volume (kg/m³), approximately 1.225 kg/m³ under standard conditions
- **Drag Coefficient**: Related to the aerodynamic properties of the bicycle and rider, dimensionless. Refer to Wikipedia and literature for settings. (According to DOVAL, Peter Nicholas. Aerodynamic analysis and drag coefficient evaluation of time-trial bicycle riders. 2012, the drag coefficient for road cyclists is approximately 0.7)
- **Frontal Area**: The projected area of the bicycle and rider facing the wind (m²)
- **Velocity**: Bicycle speed (m/s), typically converted from km/h in calculations

![Weiki](https://cdn-images-1.medium.com/max/800/1*HyFH69tUD5MTLO4Ot3v8Rg.png)
*Weiki*

Air resistance increases with the square of velocity, making it the primary source of resistance at high speeds. This is why racing bikes and aerodynamic bicycles have flattened, peculiar shapes, XD.

## 2. Rolling Resistance

Rolling resistance comes from the contact between tires and the ground. It depends on tire material, pressure, and road conditions:

```java
Rolling Resistance = Friction Coefficient × Total Weight × Gravity × cos(Slope Angle)
```

Where:

- **Friction Coefficient**: Related to tire material, pressure, and road conditions, dimensionless (Wikipedia states it’s approximately 0.0022)
- **Total Weight**: Combined weight of rider and bicycle (kg)
- **Gravity**: Usually 9.8 m/s²
- **Slope Angle**: Road gradient angle, converted from percentage grade to radians in calculations

The cosine term accounts for the slightly reduced normal force component on slopes.

## 3. Gravitational Resistance (Climbing Resistance)

When climbing, a bicycle must overcome the component of gravity parallel to the slope:

```java
Climbing Resistance = Total Weight × Gravity × sin(Slope Angle)
```

This force becomes significant on steeper gradients and is directly proportional to the combined weight.

Considering all the above data, we can determine the resistance based on slope, bicycle weight, wind resistance, etc., and further calculate the target power. Below, I’ve used a binary search algorithm to calculate how to achieve the target power.

## Binary Search Algorithm for Finding Equivalent Slopes

When testing e-bikes, we often need to find a slope that requires a specific target power. Instead of directly solving complex equations, a binary search algorithm provides a more versatile solution:

1. Start with minimum and maximum possible slope values
2. Calculate the power required at the midpoint slope
3. If the calculated power is higher than the target, decrease the slope
4. If the calculated power is lower than the target, increase the slope
5. Repeat until the calculated power converges to the target power

This numerical method is more practical because parameters like air resistance can vary with speed, making direct equation-solving challenging.

## Applications in E-Bike Testing

Understanding these physical principles is crucial for EN-15194 and R200 e-bike testing, where precise resistance calculations determine:

- Battery range estimations
- Motor assistance levels
- Performance under various conditions
- Compliance with regulatory standards

By creating accurate bicycle resistance models and using numerical methods to solve them, we can analyze bicycle performance under different conditions, optimizing both e-bike designs and testing methodologies.

## Conclusion

The physics of bicycle resistance may seem complex at first glance, but breaking it down into its component forces — air resistance, rolling resistance, and gravitational resistance — makes the calculations more approachable. Whether you’re designing e-bikes, testing them for compliance, or simply curious about bicycle physics, understanding these principles provides valuable insights into cycling performance.

This approach not only helps in standardized testing but also in designing more efficient e-bikes tailored to specific riding conditions and user needs. Based on the content above, I’ve also created an Excel macro where users can directly input settings to obtain relevant data. If you need it, please leave a message for me, and I’ll provide it to you!
