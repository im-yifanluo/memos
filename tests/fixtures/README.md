# Test fixtures

- `test-pattern.mp4`: a 9-second, 540×960 H.264 clip with a 440 Hz AAC tone, used by the upload API tests and
  `npm run test:video`. It shows SMPTE colour bars and a running timer. It was generated with GStreamer, so it is ours
  and covered by the repository's [license](../../LICENSE).

To regenerate the clip (needs GStreamer with the base, good, ugly and libav plugins):

```bash
gst-launch-1.0 -e \
  videotestsrc num-buffers=270 pattern=smpte ! video/x-raw,width=540,height=960,framerate=30/1 \
  ! timeoverlay halignment=center valignment=center font-desc="Sans Bold 40" \
  ! videoconvert ! video/x-raw,format=I420 \
  ! x264enc bitrate=400 key-int-max=30 speed-preset=slow ! video/x-h264,profile=main ! mux. \
  audiotestsrc num-buffers=90 samplesperbuffer=4410 wave=sine freq=440 volume=0.15 \
  ! audio/x-raw,rate=44100,channels=1 ! audioconvert ! avenc_aac bitrate=64000 ! mux. \
  mp4mux name=mux faststart=true ! filesink location=tests/fixtures/test-pattern.mp4
```
