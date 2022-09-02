The `docker-image-extract` script pulls and extracts all files from an image
in [Docker Hub](https://hub.docker.com/).

It has minimal dependencies that should exist in pretty much any Linux
environment, with the possible exception of `curl`/`wget`:

* `curl` or `wget`
* `find`
* `grep`
* `gzip`
* `jq`
* `mkdir`
* `rm`
* `sed`
* `sh`
* `tar`

If you are using [BusyBox](https://busybox.net/), version 1.34.0 or later is
required, as the `wget` implementation in earlier versions does not recognize
the HTTP 307/308 redirects returned by Docker Hub.

## See also

* https://rmannibucau.metawerx.net/post/docker-extracts-fileystem-with-bash

  The logic in this script is based on this blog post, but pretty much
  everything has been rewritten and improved.

* https://github.com/moby/moby/blob/master/contrib/download-frozen-image-v2.sh

  This script downloads the layers of a Docker image in a form suitable for use
  with [`docker load`](https://docs.docker.com/engine/reference/commandline/load/).
  It doesn't actually extract the layers, though.

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
