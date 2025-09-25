---
layout: post
---
To make it easier to plan and edit routes for our robot, we would like to be able to give it directions of the form "drive to `(x, y)`" instead of "turn to heading `h` and then go distance `d`."
The `DriveBase` class doesn't know how to do this; it only has methods like `turn` and `straight`.

> Question: why is it easier to plan and edit routes using coordinates?

In order to work in "coordinate space", our robot has to do two things:

1. Keep track of its current location `(x, y)`.
2. Given a current location `(x0, y0)` and a target location `(x1, y1)`, figure out how much to turn and how far to drive.

We will create our own subclass of the `DriveBase` class that enables these features. Below is a template that you can use to work on our new DriveBase class:

```python

def compute_trajectory(
  x0: float,
  y0: float,
  x1: float,
  y1: float,
) -> tuple[float, float]:
  """Given a current position and target position, compute the heading and distance.

    Args:
      x0: initial x position.
      y0: initial y position.
      x1: target x position.
      y1: target y position.

  Returns: a pair of values (h, d) representing the heading and distance to drive to go from
    (x0, y0) to (x1, y1).
  """

  # IMPLEMENT THIS FUNCTION


class ArtemisBase(DriveBase):

  def __init__(
    self,
    # *args and **kwargs are just a way of saying "collect all the inputs to this constructor".
    *args,
    **kwargs,
  ):
    # super() just means "get my parent class". So this line just says
    # "take all the arguments to my constructor and pass them to the constructor for my parent class."
    super().__init__(*args, **kwargs)

    # We assume our start location is (0, 0).
    self.reset_location()

  def reset_location(
    x: float = 0,
    y: float = 0,
  ):
    self.x = 0
    self.y = 0

  def turn_to(
    self,
    heading: float,
  ):
  """Turns the robot to face in the direction `heading`."""

    # IMPLEMENT THIS FUNCTION

  def drive_to(
    x: float,
    y: float,
  ):
  """Drives from the current location to (x, y)."""

  # IMPLEMENT THIS FUNCTION

```

## Questions
1. What happens if someone calls `.arc(...)` on an instance of `ArtemisBase`? What can we do about this?
2. How can we test that our code is correct?

