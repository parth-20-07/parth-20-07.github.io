---
id: math
aliases:
  - Interpolation
tags:
---

# Interpolation

**Table of Contents**

## Why do we need this?

Interpolation can help you construct values between two points based on available data. Common Use Case:

**Sensor Data Fusion**: Often Sensors operate at different frequency from each other, so the data you might try to collect might not match for a particular timestamp. You can use Interpolation to generate Data for the missing time stamp based on available data from previous records.

## LERP: Linear Interpolation

Assume a situation with 2 vectors *vi* and *vf* as follows:

![https://www.notion.soinit.png](https://www.notion.soinit.png)

Initial-Situation

LERP is used when you want a linear interpolation from start to end. This is the shortest distance between the two points as shown:

![https://www.notion.soLERP.png](https://www.notion.soLERP.png)

![LERP.png](LERP.png)

Assume you want to find a point on this new vector *vfi* at some distance from *vi* which is denoted by Vector *vs* from the origin.

Here,

$$
s ∈ [0,1]\\
\begin{equation}
\begin{split}
v_{s} = v_{i} + s(v_{f} - v_{i})\\
v_{s} = (1-s)v_{i} + sv_{f}
\end{split}
\end{equation}
$$

Thus,

$$
\begin{split}
\forall s=0 \Rightarrow v_{s} = v_{i}\\
\forall s=1 \Rightarrow v_{s} = v_{1}
\end{split}
$$

---

### Time Version for LERP

*Lerp*(*p*0,*p*1;*t*) = (1−*t*)*p*0 + *tp*1

for *t* ∈ [0,1]

---

> This works best in following situations:
> 

> The Interpolation is needed in a 3D or less Cartesian System where the interpolation is estimated to be a straight line.
> 

> The faster computation is essential over accuracy.
> 

> The data points are so close to each other that the ∂X can be approximated to a straight line.
> 

## SLERP: Spherical Linear Interpolation

Assume a situation with 2 vectors *vi* and *vf* on the surface of the sphere of unit distance from center of the sphere as follows:

![SLERP.png](SLERP.png)

![https://www.notion.soSLERP.png](https://www.notion.soSLERP.png)

SLERP is used when you want an interpolation along an arc between *vf* and *vi*. A hypothetical plane (Plane OFI) is constructed to image the motion that passes through the point on sphere for *vi*, *vf* and the origin of the sphere. We assume a vector *vs* along the arc on the same plane OFI.

Assume a slice from the plane as shown:

![https://www.notion.soSLERP-2.png](https://www.notion.soSLERP-2.png)

![SLERP-2.png](SLERP-2.png)

Here:

- Let the angle between *vf* and *vi* be *θ*.
- If we want to find a vector *vs* on the resultant arc, the angle of *vs* w.r.t. *vi* is ***sθ***.

*vs* = *αsvi* + *βsvf*

If we take a closer look at the defined system, we have the understanding that the vectors are all of equal magnitude with a common origin. i.e. Vector *vf* and vector *vs* can be written in terms of a rotation matrix *R* and *vi*.

$$
\begin{split}
v_{f} = v_{i}R\\
v_{s} = v_{i}R^{s}
\end{split} 
$$

where: *s* ∈ [0,1]

Thus,

$$
\begin{split}
v_{s} = \alpha_{s}v_{i} + \beta_{s}v_{f}\\
v_{i}R^{s} = \alpha_{s}v_{i} + \beta_{s}v_{i}R
\end{split}
$$

Upon solving this based on the video attached in resources, the final result seems as follows:

$$
v_{s} = \frac{\sin((1-s)\theta)}{\sin(\theta)}v_{i} +
\frac{\sin(s\theta)}{\sin(\theta)}v_{f} 
$$

---

### Time Version for SLERP

$$
Slerp(p_{0}, p_{1};t) = \frac{\sin((1-t)\theta)}{\sin(\theta)}p_{0} +
\frac{\sin(t\theta)}{\sin(\theta)}p_{1} 
$$

where, *θ* = cos−1(*p*0⋅*p*1)

for *t* ∈ [0,1]

---

### Time Version for Quaternion SLERP

*Slerp*(*q*0,*q*1;*t*) = (*q*1*q*0−1)*tq*0

for *t* ∈ [0,1]

---

> This works best in following situations: - The Interpolation is needed in a 3D or less Polar System where the interpolation is estimated to be along a curve. - This process is computationally more intensive than LERP.
> 

## Resources

- [Geometric Algebra - Linear and Spherical Interpolation (LERP, SLERP, NLERP)](https://youtu.be/ibkT5ao8kGY?si=7KkgVveXInlWPSht)
- [Wikipedia: Slerp](https://en.wikipedia.org/wiki/Slerp)
