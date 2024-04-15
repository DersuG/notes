# Keyframe Animation
- interpolate between keyframes

## Inbetweening

## Correspondences
- data/geometry must "match"
- corresponding start and end value

## Interpolating between curves
- 2 points don't map to the same point
    - line segments map to line segments (may differ in size)
- every point maps to some point

### Interpolating control points
- simple
- curves must have the same # of control points

### Supersampling
- generate same number of points on each curve, then interpolate between them

## Warping
- attenuated displacement

## 2d grid deformation
- easier to deform grid points than object vertices
- each point has a grid cell and a position in that cell
- warp each cell and the points within

## 2d skeletal deformation
- points are assigned to bones, with weights
- points have a distance along their bone, and a distance away from it

## 3d free-form deformations (FFDs)
- 3d grid
- tri-cubic interpolation (2d has bi-cubic interpolation)

## Interpolating between polyhedrons
- correspondence problem
- problem: objects with different topology may be impossible to interpolate between