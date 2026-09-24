# EKIN V1 — Science Olympiad Robot Tour Robot

> Embedded control code for **EKIN V1**, a four-wheel **mecanum-drive** autonomous robot built for the Science Olympiad **Robot Tour** event. The robot navigates an unseen, maze-like track through a sequence of gates and stops on a target point — scored not on speed, but on how precisely it hits a **requested target time** and **target position**.

---

## Table of contents

- [The event](#the-event)
- [Why mecanum drive](#why-mecanum-drive)
- [Hardware](#hardware)
- [Software overview](#software-overview)
- [How a run works](#how-a-run-works)
- [Building & flashing](#building--flashing)
- [Tuning](#tuning)
- [Repository structure](#repository-structure)
- [Results](#results)
- [Team](#team)

---

## The event

Robot Tour (Science Olympiad, 25-26) tasks one robot with autonomously driving a track, passing through each labelled **gate** in order, and coming to rest on a **target point**. The catch: scoring rewards *precision over speed*. Points are lost for missing the target time (in either direction — too fast is as bad as too slow) and for distance from the target point. The track layout is revealed only on competition day, and the team gets a short window (~10 minutes) to program the run before scoring.

This makes the event a control-and-calibration problem, not a horsepower problem: the robot has to travel **known distances and turns very repeatably**, then have its segment timing tuned on the day to land on the requested time.

## Why mecanum drive

EKIN V1 uses a **mecanum wheelbase** — four wheels with angled rollers that let the robot translate in any direction (forward, sideways/strafe, and rotate) without turning its body. Compared to the differential (two-wheel) drive most Robot Tour robots use, mecanum trades some simplicity for:

- **Strafing** — line up with a gate or target without a multi-point turn.
- **In-place correction** — small lateral nudges to fix alignment error.
- **[FILL IN any other reason the team chose mecanum — e.g. tighter maneuvering in dense gate clusters]**

The cost is more complex motion math (each of the four wheels gets an independent target) and more sensitivity to wheel slip, which the control code below has to manage.

## Hardware

> Replace this bill of materials with EKIN V1's actual parts.

| Subsystem        | Part                                   | Notes                                  |
| ---------------- | -------------------------------------- | -------------------------------------- |
| Microcontroller  | `[FILL IN — e.g. Arduino Uno / ESP32]` | C++ / Arduino toolchain                |
| Drivetrain       | 4× mecanum wheels + 4× DC motors       | `[gear ratio / wheel diameter]`        |
| Motor driver(s)  | `[FILL IN — e.g. L298N / TB6612FNG]`   | `[one per 2 motors? channel mapping]`  |
| Heading sensor   | `[FILL IN — e.g. MPU6050 IMU]`         | Gyro for accurate turns                |
| Odometry         | `[FILL IN — wheel encoders? none?]`    | How distance is measured               |
| Power            | `[FILL IN — battery pack / voltage]`   |                                        |
| Chassis          | `[FILL IN — custom / kit / 3D-printed]`|                                        |

**Wiring / pinout:** `[FILL IN — a short table or link to a wiring diagram. Even a photo of the build helps reviewers a lot.]`

## Software overview

The firmware is written in C++ ([CLion project under `soft/`]). At a high level it is organized into:

- **Motion primitives** — `[FILL IN]` functions that command the mecanum base (e.g. `driveForward(cm)`, `strafe(cm)`, `turn(degrees)`), converting a desired robot motion into the four individual wheel commands.
- **Heading/closed-loop control** — `[FILL IN — e.g. a PID loop on the IMU heading to keep drives straight and make turns land on angle]`.
- **Run sequence** — the ordered list of moves that defines the path through the gates for a given track (see below).
- **Timing/calibration constants** — the tunable values (cm-per-step, turn constants, target-time padding) adjusted on competition day.

> If the code uses PID or feedforward for velocity/heading, document the structure here — it's one of the most impressive things to a reader and worth calling out explicitly.

## How a run works

1. Power on and let sensors initialize/calibrate 
2. The robot decelerates onto the target point and stops.
3. Segment timings are tuned so total elapsed time matches the **requested target time**.
4. To start the robot, click the button with a pencil like the requirement.

## Building & flashing

> Adjust to the actual toolchain.

```bash
# Open soft/ in CLion (or your IDE of choice)
# Select the [board] target and the correct serial port
# Build and upload to the microcontroller
```

## Repository structure

```
The-EKIN-V1/
└── soft/                # C++ firmware (CLion project)
    └── [source files]   # [FILL IN — list the real .cpp/.ino/.h files once added]
```

> **Housekeeping:** `.DS_Store` should be removed and added to `.gitignore`, and committing the `.idea/` IDE folder is optional — many projects gitignore it. Neither is urgent, but both make the repo look tidier.

## Results

`[FILL IN — how did EKIN V1 place? regional/state? best time accuracy / distance accuracy achieved? A concrete result here is the single most compelling line in the whole README.]`

## Team

Built by `Shivansh Upadhyay, Aryav Agarwal` for `Brookfield Central High School Science Olympiad Team`, Science Olympiad `2025-2026`.
