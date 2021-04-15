The `docker-image-extract` script pulls and extracts all files from an image
in Docker Hub.

It has minimal dependencies that should exist in pretty much any Linux
environment, with the possible exception of `curl`/`wget`:

* `curl` or `wget`
* `grep`
* `gzip`
* `find`
* `mkdir`
* `rm`
* `sed`
* `sh`
* `tar`

## See also

* https://rmannibucau.metawerx.net/post/docker-extracts-fileystem-with-bash

  The logic in this script is based on this blog post, but pretty much
  everything has been rewritten and improved.

* https://github.com/moby/moby/blob/master/contrib/download-frozen-image-v2.sh

  This script ostensibly does something similar. It's a lot more elaborate
  and maybe handles more edge cases, but I haven't looked into the exact
  differences. It also has a dependency on `jq`, which I wanted to avoid.

## Sample output

```
$ ./docker-image-extract busybox:musl
Getting API token...
Getting image manifest for library/busybox:musl...
Downloading layer e4df39e4f349fc83694349a5a94afc8d9857fa66f7421427f3d2463a58d876d3...
Extracting layer...
Image contents extracted into ./output.
$ ls output
bin  dev  etc  home  root  tmp  usr  var
$ ls -l output/bin/{busybox,ls}
-rwx------ 393 jjlin users 1148648 Apr  7 13:18 output/bin/busybox
-rwx------ 393 jjlin users 1148648 Apr  7 13:18 output/bin/ls
```
