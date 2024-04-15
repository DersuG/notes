# Rendering

## Local vs. global illumination
- **local illumination:** limited # of light bounces (usually 1)
- **global illumination:** result of all light bounces
  - super expensive
  - usually approximated or ignored

## Algorithm
1. for each pixel in view, cast a ray to world geometry
2. for each hit, cast rays to all light sources to determine color

## Material properties
- **specular reflection:** bounced light rays

## Lighting
- `color = intensity * material`
  - yellow light `(1, 1, 0)` on a cyan object `(0, 1, 1)` appears green `(0, 1, 0)`

## Phong lighting
- `ambient + diffuse + specular`
  - `ambient` is usually a constant with a low value
  - `diffuse`
    - assumes light is equally reflected in all directions
    - uses hit angle of light (ignores hit angle of view)
    - $L_\text{diffuse} = I_\text{diffuse} * K_\text{diffuse} * cos(\theta_i)$
      - $\theta$ is angle from normal
  - `specular` is the shiny highlight spot
    - $R = 2(L \cdot N) N - L$
    - $L_\text{specular} = I_\text{specular} * K_\text{specular} * cos^n{\phi}$
    - $N$ is normal
    - $n$ is shininess coefficient
    - $\phi$ is angle between eye and bounced light ray
    - $R$ is bounced light ray

### Algorithm to color a ray
```
Color shade(R)
{
    intersect objects - get closest intersection
    c = ambient
    for each light source {
        TODO
```

## Material properties

## Shadows
- do a raycast from each light to the hit point to check for occlusion

## Reflection
- do an additional raycast from the hit point using the bounced view vector
  - if it hits nothing, add the background light to the color
  - if it hits something, determine that object's color and add it to the color
  - can chain multiple bounces if you want
  - can bounce until you hit the background light, but should have a cap

## Refraction
- ray changes direction based on material and hit angle

## FOV parameters
- $\text{aspect ration} = \frac{y}{x} = \frac{tan(\text{vertical FOV} / 2)}{tan(\text{horizontal FOV} / 2)}$

