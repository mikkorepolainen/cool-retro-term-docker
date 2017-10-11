# Dockerized cool-retro-term

Docker image for running [cool-retro-term](https://github.com/Swordfish90/cool-retro-term) in a docker container.

## Build

`docker build -t mikkorepolainen/cool-retro-term docker`

## Usage

`docker run --rm -e DISPLAY=<host>:0 --hostname crt mikkorepolainen/cool-retro-term`

where `<host>` is empty for localhost or an IP for the computer where the desired X server is running.

To mount a data volume, add `-v C:\Some\Path:/some/path` to the command before the image name.

Keep your configuration by adding a volume for the internal sqlite db: `-v cool-retro-term-config:/root/.local/share/cool-retro-term/QML/OfflineStorage/Databases/` (use a bind mount like for data volumes if you want to muck around with the db externally).

Append arguments at the end of the command, e.g. `docker run --rm -it mikkorepolainen/cool-retro-term -h` to display usage.

## Locally on Linux

You can execute `xhost +` before running to turn off access control for X (perhaps not necessary, probably unadvisable?) and the X connection is managed by mounting the X11 socket as a volume (I'll just pretend I know what that means).

`xhost + && docker run --rm -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY mikkorepolainen/cool-retro-term`

TODO not tested

## On Windows (10) with Docker for Windows

Install an X server, e.g. [VcXsrv Windows X Server](https://sourceforge.net/projects/vcxsrv/) or [Xming](https://sourceforge.net/projects/xming/).
The `<host>` in the `docker run` command should point to the local machine's IP address because `localhost` points to the docker engine virtual machine instead of the local machine.

Don't be disappointed because of the terrible frame rate though... I have no clue whether it could be improved.

### VcXsrv

VcXsrv versions above 1.14.3 have problems with glx on some systems, [see here](https://sourceforge.net/p/vcxsrv/bugs/19/).
The following arguments (defaults) with version 1.14.3 seem to work: `"C:\Program Files\VcXsrv\vcxsrv.exe" :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl`

### Xming

The last public domain version available at the time of writing is 6.9.0.31 from august 2016. I have not tried the commercial version if one indeed exists.

The following arguments should be used (tweak the launcher or add the xming executable to path): `xming :0 -ac -clipboard -multiwindow`.
The terminal executable complains about unsupported glx 1.3 but it works nevertheless.

> WARNING: Application calling GLX 1.3 function "glXCreatePbuffer" when GLX 1.3 is not supported!  This is an application bug!

# References

- <http://somatorio.org/en/post/running-gui-apps-with-docker/>
- <https://blog.jessfraz.com/post/docker-containers-on-the-desktop/>
