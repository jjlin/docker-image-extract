The `docker-image-extract` script pulls and extracts all files from an image
in Docker Hub.

It has minimal dependencies that should exist in pretty much any Linux
environment, with the possible exception of `curl`:

* `curl`
* `grep`
* `gunzip`
* `find`
* `mkdir`
* `rm`
* `sed`
* `sh`
* `tac`
* `tar`
* `uniq`

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
Downloading layer b1911a4512165fe8d97b0871bd237ff314b1c7d39366bc295c3e0ba93bde7a61...
Extracting layer...
Downloading layer a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4...
Extracting layer...
Image contents extracted into ./output.
$ ls output
bin/
dev/
etc/
home/
root/
tmp/
usr/
var/
$ ls -l output/bin/{busybox,ls}
-rwx------ 392 jjlin users 1165544 Apr 13 16:43 output/bin/busybox
-rwx------ 392 jjlin users 1165544 Apr 13 16:43 output/bin/ls
```
