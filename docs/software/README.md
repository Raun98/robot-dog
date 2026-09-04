# Software

Planning for the Raspberry Pi 5. Code later lives in `/software`.

## OS and Hailo

- Raspberry Pi OS **64-bit** (Bookworm or the current recommended image at install time).
- Install Hailo stack with `sudo apt install hailo-all` after the AI HAT+ is seated; reboot.
- Enable **PCIe Gen 3** for throughput (AI HAT+ is often already Gen 3; confirm).
- Sanity: `hailortcli fw-control identify`, `rpicam-hello -t 10s`.
- Official examples: [hailo-rpi5-examples](https://github.com/hailo-ai/hailo-rpi5-examples). Pin the HailoRT version that matches the kernel; mismatched driver/firmware is a common break.

Vision inference **defaults to Hailo**, not CPU YOLO.

## Vision pipeline (v1)

1. CSI Camera Module 3 → libcamera / `rpicam`.
2. Hailo HEF (start from a supported object-detection demo, e.g. YOLO-class on Hailo).
3. Post-process: boxes, class, score → behavior process (Unix socket, ZeroMQ, or a small ROS 2 node later).
4. Behavior: track a person or a colored target; **no** aggressive chase in v1 ([safety](../safety/README.md)).

USB camera is fallback if CSI routing fails; Hailo demos prefer CSI.

## Audio pipeline (v1)

Pi 5: **USB or I2S only**.

- Wake word: lightweight always-on (Porcupine-class or a small open-source keyword model on CPU).
- STT: on-demand (Whisper-tiny / streaming STT) so CPU does not fight Hailo + behavior all the time.
- TTS: optional; USB speaker.

Hailo for audio models is optional later if a compiled HEF exists; v1 does not depend on it.

## Behavior and comms

- Process on the Pi at ~10–20 Hz: fuse vision events + voice commands → **mode + body velocity** to the MCU.
- Serial protocol: documented in firmware docs; software must honor e-stop and connection-loss (stop sending walk; MCU fails safe).
- **ROS 2** (Humble/Jazzy on Pi) is **optional**. v1 can be Python processes + systemd. Revisit ROS 2 if multiple developers or Nav2-style stacks appear.

## What not to run on the Pi

- 12-channel analog servo PWM as the production loop.
- Large LLMs as the inner gait controller.

## Bring-up order (Phase 2–3)

1. Headless SSH, cooler, `hailo-all`, camera demo.
2. Detection demo at acceptable FPS.
3. USB mic wake-word proof.
4. Serial echo to MCU.
5. Only then couple perception to motion modes.
