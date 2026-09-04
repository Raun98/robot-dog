# Architecture

Working assumption until a new ADR says otherwise. Compute is fixed ([ADR-0001](../decisions/0001-compute.md)). Motion split is [ADR-0002](../decisions/0002-motion-stack.md).

## Block diagram

```text
[CSI camera] --> [Pi 5 + AI HAT+ Hailo] --> behavior (see, hear, decide)
[USB/I2S mic] --> [Pi 5 CPU / optional Hailo] --/
                      |
                      | USB-serial or UART (commands + telemetry)
                      v
              [motion MCU] --> [PCA9685 + 12x MG995 (v1)]
                      ^
                      |
              [IMU]   [e-stop]
```

## Process split

| Layer | Where | Rate (order of magnitude) | Responsibility |
| --- | --- | --- | --- |
| Vision | Hailo on AI HAT+ | 10–30+ FPS depending on model | Detect / track; boxes to behavior |
| Audio | Pi CPU first | Wake word continuous; STT on demand | Hear commands |
| Behavior | Pi 5 | 10–20 Hz | Map perception + commands to gait modes |
| Gait + IK | Motion MCU | 50–100+ Hz | Joint trajectories, IMU stabilize |
| Servos | PCA9685 PWM (v1) | ~50 Hz PWM | Track joint setpoints (open loop) |

Do **not** bit-bang twelve analog servos from Python on the Pi in production. Do **not** run Hailo vision on the Uno.

## Interfaces

- **Pi → MCU:** framed serial (length + CRC). Messages: mode (stand/walk/estop), body velocity, optional joint overrides. Telemetry back: MCU faults; joint positions only if bus servos are added later.
- **Camera:** CSI (CAM1) into Pi 5, `rpicam` / libcamera into Hailo post-process. USB camera only if CSI routing fails.
- **Audio:** USB mic or I2S MEMS + USB/I2S DAC. Pi 5 has no analog headphone jack.
- **HAT stack:** AI HAT+ owns PCIe and sits on the 40-pin header. Extra boards are **not** stacked HATs; they are USB or flying-lead GPIO.

## Failure modes

| Failure | Desired response |
| --- | --- |
| Hailo/PCIe drop | Vision disables; robot stands or sits; log and require reboot |
| Serial to MCU lost | MCU holds last safe pose or damps to sit; no walk |
| Servo stall / binding (MG995 has no current telemetry) | Conservative PWM; hardware e-stop; later bus servos can report current |
| Pi brownout | Hardware undervoltage; servos must not share the Pi 5V rail |
| E-stop pressed | Immediate PWM/bus disable; software cannot override without reset |

## Reference designs (cite, do not clone)

Mini Pupper, SpotMicro, Stanford Pupper, CHAMP / ROS 2 quadruped stacks. Links: [references](../research/references.md).
