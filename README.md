# Traffic Light Controller — FSM & Proteus

Digital logic design of a three-state traffic light controller with BCD countdown displays, developed as a university group project.

**Portfolio owner:** Thái Trung Sinh · **Focus:** FSM design, Karnaugh maps, sequential logic, Proteus simulation

[Tiếng Việt](README.vi.md) · [Design details](docs/design.md) · [Technical slide excerpts](docs/technical-slides.pdf) · [Proteus project](proteus/TrafficLight.pdsprj)

![Traffic light controller schematic from the project presentation](assets/proteus-schematic.png)

*Original Proteus circuit image extracted from slide 25. The image records the project presentation; it is not a new simulation run.*

## My contribution

This repository documents my contribution to **Group 2's traffic light project**. My responsibilities, as listed in my CV, were:

- Developed the state table and digital logic for the traffic light controller.
- Designed a **three-state FSM** and simplified logic using **Karnaugh maps**.
- Combined **74192 BCD down counters** with **seven-segment displays** to show the remaining count.
- Designed and simulated the controller circuit in **Proteus**.

The presentation and complete circuit are shared group deliverables. This portfolio highlights my responsibilities within that work.

## Overview

The controller cycles through **Green → Yellow → Red → Green**. A shared two-digit countdown branch serves the green and red phases, while a separate one-digit branch serves the yellow phase. The FSM selects the active branch and drives the three light outputs.

| Block | Purpose |
| --- | --- |
| Three-state FSM | Selects the light phase and counter enable signals |
| Three 74192 counters | Two BCD digits for the green/red branch; one for yellow |
| 74LS247 decoders | Convert BCD outputs to seven-segment display signals |
| Seven-segment displays | Show countdown values |
| 4013 flip-flops and logic gates | Store the FSM state and implement control logic |

The component names above follow the supplied Proteus project. The slides also discuss the 7447/74LS47 decoder family.

![Original project block diagram](assets/block-diagram.png)

## FSM design

The state encoding follows the presentation's `s0 s1` convention. `C1` indicates completion of the green/red countdown; `C2` indicates completion of the yellow countdown.

| State | Code (`s0 s1`) | Light on | `en1` | `en2` | Transition |
| --- | --- | --- | --- | --- | --- |
| A | `00` | Green | 1 | 0 | Stay if `C1=0`; go to B if `C1=1` |
| B | `01` | Yellow | 0 | 1 | Stay if `C2=0`; go to C if `C2=1` |
| C | `10` | Red | 1 | 0 | Stay if `C1=0`; go to A if `C1=1` |

![Original three-state FSM diagram](assets/fsm-diagram.png)

The state table and Karnaugh maps document the transition logic and output decoding. See [design details](docs/design.md) for the equations and supporting figures.

## Open the project

1. Download or clone the repository.
2. Open [`proteus/TrafficLight.pdsprj`](proteus/TrafficLight.pdsprj) in a compatible Proteus installation with the required component models.
3. Inspect the clock sources, counter preset inputs and initial FSM state.
4. Run the simulation and observe the countdown displays and light sequence.

The original project file is preserved without modification. The supplied materials document the design and its presentation; the simulation has **not been rerun as part of preparing this repository**. No fixed phase duration or clock frequency is asserted here.

## Repository contents

| Path | Contents |
| --- | --- |
| [`proteus/TrafficLight.pdsprj`](proteus/TrafficLight.pdsprj) | Original Proteus project |
| [`assets/`](assets/) | Circuit, FSM, table and Karnaugh-map images extracted from the slides |
| [`docs/design.md`](docs/design.md) | State table, equations and evidence notes |
| [`docs/technical-slides.pdf`](docs/technical-slides.pdf) | Six technical slide excerpts (original pages 5 and 21–25) |
| [`README.vi.md`](README.vi.md) | Vietnamese project description |

## Skills demonstrated

Finite-state machine design · State encoding · Boolean simplification · Karnaugh maps · BCD counters · Seven-segment decoding · Gate-level circuit simulation

## Project context

Developed as part of Group 2 university coursework. The repository describes my individual contribution within the group project.
