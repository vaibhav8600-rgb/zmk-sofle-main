# Custom splash

Drop a **PNG** here and point `config/nexus_dongle.conf` at it:

```
CONFIG_NEXUS_SPLASH_IMAGE="nexus/splash/splash.png"
CONFIG_NEXUS_SPLASH_MAX_DIM=240
```

The build converts it to RGB565 and links it instead of the built-in badge.
Nothing is drawn over it - no wordmark, no brand line, no artwork. The badge's
text lives inside the badge's own draw function, and your image replaces that
function entirely.

PNG only. `scripts/png2c.py` in the NEXUS module is stdlib-only on purpose
(Pillow is not in ZMK's build container), so it reads 8-bit grayscale,
grayscale+alpha, RGB, RGBA and palette PNGs. Interlaced PNGs are rejected
rather than mangled. `magick photo.jpg splash.png` converts anything.

240x240 costs 115,200 bytes of flash - 14.2% of the app partition. The build
prints the exact figure. `MAX_DIM` scales the image down to fit, by any ratio:
216 is 93,312 bytes.

This dongle's bootloader (Adafruit UF2 0.6.0) will not write at or above
`0xAC000`, so the app has about 536 KB, not the 792 KB CI reports. With Buddy
on, 240 does not fit and 216 does - which is what `nexus_dongle.conf` sets.
A firmware that is too big copies "fine", but the NICENANO drive never goes
away and the screen stays blank.
