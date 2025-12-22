# Issues and Fixes

## RTSP Stale Frames
- Symptom: Robot stopped responding although video was visible
- Root cause: OpenCV internal buffering
- Fix: Threaded capture + grab/retrieve strategy

## Torch Import Error (libucc.so)
- Symptom: ImportError: undefined symbol
- Root cause: HPCX library conflict
- Fix: Custom LD_LIBRARY_PATH configuration

## RTDE Connection Timeout
- Symptom: servoL fails after a while
- Root cause: Loose Ethernet cable
- Fix: Physical inspection + reconnect handling
