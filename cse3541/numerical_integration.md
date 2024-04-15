# Numerical Integration

## Recording motion
- save position over time
- then look up position given time
- problem: if sampling rate is too slow, important details may be missed

## Kinematics
- derivatives: position -> velocity -> acceleration
- integrals: position <- velocity <- acceleration

## Euler integration
- for arbitrary $f(t_i)$ with known derivative $f'(t_i)$
- step in direction of derivative
- $f(t_{i+1}) = f(t_i) + f'(t_i) \cdot \Delta t$
- problems:
    - works for constant acceleration
    - changing acceleration introduces errors
        - the larger the step, the more time passes where we aren't accounting for differences in the changing acceleration

## Runge Kutte integration
- "2nd order method" aka "midpoint method"
- do a full Euler step
- evaluate $f'$ at midpoint
- take a step from the original point using the midpoint's $f'$ value
- $f(t_{i+\frac{1}{2}}) = f(t_i) + \frac{1}{2} f'(t_i) \cdot \Delta t$
- $f(t_{i+1}) = f(t_i) + f'(t_{i + \frac{1}{2}}) \cdot \Delta t$

## Implicit Euler integration

## Huen integration

## Verlet integration

## Leapfrog integration