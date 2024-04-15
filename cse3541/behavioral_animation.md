# Behavioral Animation
- control objects with virtual actors
- motion based on:
  - knowledge of environment
  - aggregate behavior
  - primitive behavior
  - intelligent behavior
  - crowd behavior

## Actor properties (position only)
- V = goal - position
- position += V * delta

## Problems with repulsive forces
![[Pasted image 20240212114627.png]]

## Types of motion behavior
- action selection
- steering
- locomotion

## Reflex agent
![[Pasted image 20240212114712.png]]

## Actor properties (position + orientation)
- rotates towards the target

```c#
class OrientedAgent2D
{
  GameObject model;

  Update(float deltaTime);
  TurnLeft();
  TurnRight();
  MoveForward();
  MoveBackward();
}
```

## Knowing the environment

### Vision
- brute force: loop over all objects, use raycasts
- **omniscience:** everything in the scene is known
- **distance-limited omniscience:** yeah
- **field of view:** check if angle is within limits
- **occluded vision:** check if stuff is in the way
- **target-testing vision (per-object):** use raycasts to check if we can see a target
  - can use multiple to check how well the actor can see

### Memory
- info stored from the past (e.g. last known player position)