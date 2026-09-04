# Architecture

Working assumption: [ADR-0004](../decisions/0004-hardware-freeze.md) (stack), [ADR-0001](../decisions/0001-compute.md) (compute), [ADR-0005](../decisions/0005-mechanical-layout.md) (body). Motion history: [ADR-0002](../decisions/0002-motion-stack.md).

## Block diagram

```text
[Camera Module 3 CSI] --> [Pi 5 + AI HAT+] --> behavior
[USB mic] --------------^
        USB-serial
            v
[ESP32-S3] --I2C + level shift--> [PCA9685] --> 12x MG995
    BNO055 --I2C --^
    ACS712 30A on servo rail
    e-stop cuts servo rail (ATX 5V bench / 6V BEC later)
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

- **Pi → MCU:** framed serial to **ESP32-S3**. Modes: stand/walk/estop, body velocity. Telemetry: IMU attitude, rail current, faults. No joint encoders on MG995.
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
