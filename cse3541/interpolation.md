# Interpolation

## Path smoothing
- problem: some sharp corners are important
![[Pasted image 20240228113946.png]]

## Interpolation terms
- **order:** (of polynomial), e.g. linear, quadratic, cubic
- **dimensions:** e.g. barycentric (within a triangle), bilinear, trilinear

## Linear interpolation between 2 points
- $P = (1 - t)P_0 + tP_1$
    - $x = (1-t)x_0 + tx_1$
    - $y = (1-t)y_0 + ty_1$

## Bilinear interpolation
- given 4 points ("Q's")
- each axis has its own $t$ value ($t$ for x, $s$ for y)

## Lab 5 notes
1. construct a space curve $p = P(u)$ that interpolates the given points with piecewise first order continuity
2. construct an arc-length-parametric-value function $u = U(s)$ for the curve
    1. supersampling
3. construct a time-arc-length function $s = S(t)$ according to the given constraints
    1. easing function
4. $p = P(U(S(t)))$

## Choosing interpolation functions
- interpolation vs. approximation
    - **interpolating function:** guarantees the path will pass through the points
    - **approximating function:** path is only affected by points
- polynomial complexity: cubic
    - having a point of inflection allows the curve to match arbitrary tangents at endpoints
- continuity: first degree (tangential)
    - **continuity order:**
        - $C^{-1}$: discontinuous
        - $C^0$: continuous
        - $C^1$: first derivative is continuous
        - $C^2$: second derivative is continuous
        - it's the number of derivatives you can take before becoming discontinuous
- local vs. global control: local
    - **local control:** control points only change the curve over a finite region
    - **global control:** control points affect the entire curve
- information requirements: do you need tangents?
    - only points
    - tangents
    - interior control points
    - only beginning and ending tangents

## Lagrange polynomial
![[Pasted image 20240228122506.png]]
- problem: little control over direction through points
![[Pasted image 20240228122558.png]]
