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
prints the exact figure.
