# Python3 for Robotics: Go-to-Target Simulation

A small Python simulation of a differential-drive robot driving to a target point. A proportional controller turns the robot toward the target, then drives it forward until it is within 1 cm.

## How it works

The robot starts at a position and heading you choose and is given a target (x, y). On each 1 ms step the controller reads the robot pose and computes two speeds:

- **Rotational speed:** `w = Kw * angle_error`, where the angle error is the difference between the direction to the target and the robot heading.
- **Forward speed:** `vf = Kv * distance_to_target`, but only when the heading error is under 5 degrees. Otherwise the robot rotates in place until it is aligned.

Both speeds are clamped to the robot limits (0.3 m/s forward, +/-0.3 rad/s rotation). The simulation ends when the robot is within 1 cm of the target, or after a 300 s safety timeout.

## Files

| File | Role |
|---|---|
| `main.py` | Entry point: asks for settings, runs the simulation loop, saves the plots |
| `RobotClass.py` | `Robot`: pose, speed limits and position update over time |
| `ControllerClass.py` | `Controller`: proportional controller (distance, angle error, speeds) |
| `UserInterface.py` | `UserInterface`: prompts for the robot, target and gains, and prints results |
| `plotter.py` | `plot_results`: trajectory, speeds and error plots, saved to `results.png` |

## Requirements

Python 3 and Matplotlib (only needed for the plots; the simulation runs without it).

```bash
pip install matplotlib
```

## Running

```bash
python main.py
```

You will be asked for the robot name, initial x, y and heading (degrees), the target x, y, and the gains Kv and Kw. Press Enter to accept the default shown in brackets (start at the origin, target at (0.5, 0.5), both gains 1.0).

When the run finishes, the final position, distance error and elapsed time are printed, and a four-panel plot is saved as `results.png`:

1. Trajectory with heading arrows, start and target
2. Forward speed over time
3. Rotational speed over time
4. Distance error and angle error over time

Set `SHOW_PLOTS = True` in `main.py` to also open the plot window.

## Example results

The PNG files in this folder are saved plots from runs with different settings:

| File | Run |
|---|---|
| `Default.png` | Default settings |
| `StraightAhead.png` | Target straight ahead of the robot |
| `BehindTheRobot.png` | Target behind the robot |
| `NonZeroStart.png` | Start position other than the origin |
| `LowerGains.png` | Lower Kv and Kw |
| `HigherGains.png` | Higher Kv and Kw |
