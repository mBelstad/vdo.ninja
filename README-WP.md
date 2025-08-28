# Media Stack (VDO.Ninja + MediaMTX) – WordPress Embeds and Operations

This guide shows how to use the media stack entirely from WordPress.

## 1) Admin pages (Elementor HTML widgets)

Director Console:
```html
<iframe allow="camera;microphone;display-capture;autoplay;fullscreen" src="https://studio.itagenten.no/director?mediadomain=media.itagenten.no" style="width:100%;height:85vh;border:0"></iframe>
```

Breakouts:
```html
<iframe src="https://studio.itagenten.no/breakouts" style="width:100%;height:80vh;border:0"></iframe>
```

Overlay Control:
```html
<iframe src="https://studio.itagenten.no/overlay-control" style="width:100%;height:80vh;border:0"></iframe>
```

## 2) Mixer (admin only)
```html
<iframe allow="camera;microphone;display-capture;autoplay;fullscreen" src="https://studio.itagenten.no/mixer?room={{ROOM}}&password={{PASSWORD}}&whippush=https%3A%2F%2Fmedia.itagenten.no%2Fwhip%2Fmixer&whipoutvideobitrate=6000&stereo" style="width:100%;height:90vh;border:0"></iframe>
```

## 3) Presenter/guest join (green room)
```html
<iframe allow="camera;microphone;display-capture;autoplay;fullscreen" src="https://studio.itagenten.no/?room={{ROOM}}&password={{PASSWORD}}" style="width:100%;height:85vh;border:0"></iframe>
```

## 4) Viewer (HLS recommended)
```html
<iframe allow="autoplay;fullscreen" src="https://studio.itagenten.no/hls.html?src=https%3A%2F%2Fmedia.itagenten.no%2Fhls%2Fmixer%2Findex.m3u8&autoplay" style="width:100%;height:60vh;border:0"></iframe>
```

## 5) Overlay Output (choose chroma or transparent)
Chroma:
```html
<iframe src="https://studio.itagenten.no/overlay-output?chan=ACME-OVERLAY&type=lowerthird&bg=chroma" style="width:100%;height:60vh;border:0"></iframe>
```
Transparent:
```html
<iframe src="https://studio.itagenten.no/overlay-output?chan=ACME-OVERLAY&type=lowerthird&bg=transparent" style="width:100%;height:60vh;border:0;background:transparent"></iframe>
```

## 6) Viewer chat and polls (recommendation)
- Use a WordPress chat/Q&A plugin on the viewer page; speakers read/respond via a separate admin page.
- For polls/Q&A widgets, embed provider iframes in a private operator page and share the tab into the Mixer when needed.

## 7) SRT cameras
- Configure cameras to push to `srt://media.itagenten.no:8890?streamid=cam1` (or cam2..cam5).
- The Mixer publishes to MediaMTX via WHIP; audience views HLS at `/hls/mixer/index.m3u8`.

## 8) Tips
- Prefer HLS for audience; use WHEP only for low-lat monitors.
- Keep room and password in JetEngine fields; render iframes with those values.
- Use Overlay Control to manage on-air graphics; share Overlay Output tab in the Mixer.


