# Clean Debian Package Build Environment Using Docker

## Overview

With Docker, you can set up a clean build environment for Debian
packaging purposes. The container is using Ubuntu 18.04 as the base image.

Assuming that you already have the packaging files to build a `deb` package.

## Example For Building Golang Project

Modified `Dockerfile-golang` contains necessary tool to build a Go project.

Start by building the container for the build environment:

`$ docker build -t golang-deb-builder -f Dockerfile-golang .`

You should prepare `debian` directory with the necessary
packaging files: `control`, `copyright`, `changelog`, `rules`, etc.

Assuming you have everything ready, put your project and packaging files in
the current directory and start the container:

`$ docker run --rm -v "$PWD:/go" -it golang-deb-builder`

Your directory structure should look something like this:
```
/go
---
.
|-- bin
|-- debian
|   |-- changelog
|   |-- compat
|   |-- control
|   |-- copyright
|   |-- docs
|   |-- files
|   |-- rules
|   `-- source
|-- pkg
|   `-- dep
`-- src
    `-- github.com
        `-- williamchanrico
            `-- some-project
```

`$ dpkg-buildpackage -us -uc`

Sometimes build might require dependencies, so satisfy all the dependency outside
the container first or modify the `Dockerfile` to support the dependency manager tool.