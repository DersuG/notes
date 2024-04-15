# Collision Response
## Determining time/position of impact
- options:
  - resolve collision based on new position
  - figure out when the collision actually occurred and resolve based on that intermediate position

## Conservation of momentum
- $p = mv$
- $m_iv_i = p = m_fv_f$
- **elastic collision:** no loss of kinetic energy
- **inelastic collision:** loss of kinetic energy (e.g. deformation, heat)

## Elastic collision with static axis-aligned planes
- negate the corresponding velocity component
- preserve the other components

## Elastic collision with static angled planes
- split velocity into components with respect to the plane's normal
- normal component is $u = n \frac{v \cdot n}{n \cdot n}$
  - negate it
- other components are $w = v - u$