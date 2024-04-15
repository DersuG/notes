# Collision Detection
- detection
- response
- convex polygons are more efficient
  - for any 2 points inside, the line between them is fully contained
  - all angles are less than 180 degrees
- **polygon triangulation:** split polygons into several triangles

## Equation formats
- **explicit:** $y = 2x + 5$
- **implicit:** $0 = f(x, y)$
- **parametric:** $(x, y) = \left( f(t), g(t) \right)$

## Parametric line equation
- $P(t) = p_0 + tV$
  - $P_0$ is the start point
  - $V$ is the slope
  - $P_1$ is wherever $t = 1$
- can also be expressed in components:
  - $x = P_0x + t_x(P_1x - P_0x)$
  - $y = P_0y + t_y(P_1y - P_0y)$

## Point and line intersection
- solve for $t$ given $P(t)$ and $P_0$

## Line and line intersection
- make sure lines aren't parallel
- solve for intersecting point (as if lines were infinite)
  - $P_0 + t_a(P_1 - P_0) = P_2 + t_b(P_3 - P_2)$
- check if intersecting point is inside both lines
  - $0 \le t_a \le 1$
  - $0 \le t_b \le 1$

## Point in polygon intersection (ray test)
- count # of intersections between a ray starting at the point (direction doesn't matter) and the polygon edges
- if # is odd, the point is inside
- special cases:
  - ray collides with a corner
  - ray coincides with an edge

## Point in convex polygon intersection
- test if point is to the left or right of a line:
  - $(y - y_0)(x_1 - x_0) - (x - x_0)(y_1 - y_0)$
  - > 0: point is to the left of the line
  - < 0: point is to the right of the line
  - = 0: point is on the line

## Polygon intersection
- check for contained vertex and intersecting edges

![[Pasted image 20240207114240.png]]
## Point to ground plane intersection
- just check the point's y value

## Point to arbitrary plane intersection
- point: $p = (x, y, z)$
- plane:
  - normal: $N = (a, b, c)$
  - displacement: $d$
- $E(p) = ax + by + cz + d = N \cdot p + d$
- check if if $E(p) > 0$

![[Pasted image 20240207115227.png]]

## 3d object intersection
- check for contained vertex or intersecting edges/faces

![[Pasted image 20240207115606.png]]

### Order tests by cost
1. test bounding volumes
2. test for contained vertices
3. test for edge-face intersection

## Bounding volumes
- sphere
- axis aligned bounding box
- oriented bounding box
- convex hull
- bounding slabs
  - parallel planes bounding an object
- usually defined manually

### Constructing bounding volumes
- sphere: find some kind of average position, then determine radius by finding farthest point
- axis aligned bounding box: find max and min x/y/z values
- convex hull: "gift-wrapping algorithm"

## Sphere to sphere intersection
- check distances between points, compare to sum of radii

## AABB to AABB intersection
- check if x ranges overlap
- check if y ranges overlap

## OBB to OBB intersection
- and arbitrary polygons
- separating axis theorem

## Sphere to AABB intersection
- find closest point on AABB
- check distance

![[Pasted image 20240207121723.png]]

