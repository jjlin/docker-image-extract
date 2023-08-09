The `docker-image-extract` script pulls and extracts all files from an image
in [Docker Hub](https://hub.docker.com/). For multi-platform images, you can
choose the platform-specific image to pull.

`docker-image-extract` has minimal dependencies that should exist in pretty
much any Linux environment, with the possible exception of `curl`/`wget`:

* `curl` or `wget`
* `find`
* `grep`
* `gzip`
* `mkdir`
* `rm`
* `sed`
* `sh`
* `tar`

If you are using [BusyBox](https://busybox.net/), version 1.34.0 or later is
required, as the `wget` implementation in earlier versions does not recognize
the HTTP 307/308 redirects returned by Docker Hub.

## Usage

```
./docker-image-extract [OPTIONS...] IMAGE[:REF]

IMAGE can be a community user image (like 'some-user/some-image') or a
Docker official image (like 'hello-world', which contains no '/').

REF is either a tag name or a full SHA-256 image digest (with a 'sha256:' prefix).
The default ref is the 'latest' tag.

Options:

  -p PLATFORM  Pull image for the specified platform (default: linux/amd64)
               For a given image on Docker Hub, the 'Tags' tab lists the
               platforms supported for that image.
  -o OUT_DIR   Extract image to the specified output dir (default: ./output)
  -h           Show help with usage examples

Examples:

# Pull and extract all files in the 'hello-world' image tagged 'latest'.
$ ./docker-image-extract hello-world:latest

# Same as above; ref defaults to the 'latest' tag.
$ ./docker-image-extract hello-world

# Pull the 'hello-world' image for the 'linux/arm64/v8' platform.
$ ./docker-image-extract -p linux/arm64/v8 hello-world

# Pull an image by digest.
$ ./docker-image-extract hello-world:sha256:90659bf80b44ce6be8234e6ff90a1ac34acbeb826903b02cfa0da11c82cbc042
```

## Sample output

```
$ ./docker-image-extract busybox:musl
Getting API token...
Getting image manifest for library/busybox:musl...
Fetching and extracting layer e4df39e4f349fc83694349a5a94afc8d9857fa66f7421427f3d2463a58d876d3...
Image contents extracted into ./output.
$ ls output
bin  dev  etc  home  root  tmp  usr  var
$ ls -l output/bin/{busybox,ls}
-rwx------ 393 jjlin users 1148648 Apr  7 13:18 output/bin/busybox
-rwx------ 393 jjlin users 1148648 Apr  7 13:18 output/bin/ls
```

## See also

* https://rmannibucau.metawerx.net/post/docker-extracts-fileystem-with-bash

  The logic in this script is based on this blog post, but pretty much
  everything has been rewritten and improved.

* https://github.com/moby/moby/blob/master/contrib/download-frozen-image-v2.sh

  This script downloads the layers of a Docker image in a form suitable for use
  with [`docker load`](https://docs.docker.com/engine/reference/commandline/load/).
  It doesn't actually extract the layers, though.
