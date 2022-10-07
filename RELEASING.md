# Release process

## Update the code

Follow the instructions in `UPDATING.md` to update the code.

## Update the package version

If there is no API breakage, update the minor version (X.y -> X.y+1). If there
is API breakage, update the major version (X.y -> X+1.0).

## Make sure builds are successful on all platforms

There is CI for `x86_64` and `aarch64` builds, but 32-bit ARM and MIPS builds
need manual verfification (or testing downstream).

## Tag the release

Tag the release with:

```sh
git tag -s -m 'WebRTC AudioProcessing v<X.y>' v<X.y>
```

## Make a tarball

```sh
git archive --format 'tar.gz' \
  --prefix 'webrtc-audio-processing-X.y/' -9 vX.y \
  > webrtc-audio-processing-X.y.tar.gz
```

## Do a test build

```sh
tar xvf webrtc-audio-processing-X.y.tar.gz
cd webrtc-audio-processing-X.y
meson . build -Dprefix=$PWD/install
ninja -C build
ninja -C build install
cd ..
```

## Checksum the tarball

```sh
sha256sum webrtc-audio-processing-X.y.tar.gz \
  > webrtc-audio-processing-X.y.tar.gz.sha256
```

## Publish the files

```sh
scp webrtc-audio-processing-*.tar.* \
  annarchy.freedesktop.org:/srv/www.freedesktop.org/www/software/pulseaudio/webrtc-audio-processing/
```

## Push the tag

```sh
git push origin master
git push origin vX.y
```

## Update the website

This is currently an embarrassing manual process.

## Send out a release announcement

This goes to the `pulseaudio-discuss` and `gstreamer-devel` mailing lists, and
possibly `discuss-webrtc` if it seems relevant.
