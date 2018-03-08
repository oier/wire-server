# Wire™

[![Wire logo](https://github.com/wireapp/wire/blob/master/assets/header-small.png?raw=true)](https://wire.com/jobs/)

This repository is part of the source code of Wire. You can find more information at [wire.com](https://wire.com) or by contacting opensource@wire.com.

You can find the published source code at [github.com/wireapp/wire](https://github.com/wireapp/wire).

For licensing information, see the attached LICENSE file and the list of third-party licenses at [wire.com/legal/licenses/](https://wire.com/legal/licenses/).

No license is granted to the Wire trademark and its associated logos, all of which will continue to be owned exclusively by Wire Swiss GmbH. Any use of the Wire trademark and/or its associated logos is expressly prohibited without the express prior written consent of Wire Swiss GmbH.

## Wire server

This repository contains the source code for the Wire server. It contains all libraries and services necessary to run Wire.

Self hosting and federation is on our long term roadmap.

See more in "[Open sourcing Wire server code](https://medium.com/@wireapp/open-sourcing-wire-server-code-ef7866a731d5)".

## Content of the repository

This repository contains the following source code:

- **services**
   - **nginz**: Public API Reverse Proxy (Nginx with custom libzauth module)
   - **galley**: Conversations and Teams
   - **brig**: Accounts
   - **gundeck**: Push Notification Hub
   - **cannon**: WebSocket Push Notifications
   - **cargohold**: Asset (image, file, ...) Storage
   - **proxy**: 3rd Party API Integration
- **tools**
   - **api-simulations**: Run automated smoke and load tests
   - **makedeb**: Create Debian packages
   - **bonanza**: Transform and forward log data
- **libs**: Shared libraries

It also contains

- **build**: Build scripts and Dockerfiles for some platforms
- **deploy**: (Work-in-progress) - eventually this will contain instructions on how to run wire-server
- **doc**: Documentation

## Architecture Overview

The following diagram gives a high-level outline of the (deployment) architecture
of the components that make up a Wire Server as well as the main internal and
external dependencies between components.

![wire-arch](doc/arch/wire-arch-2.png)

Communication between internal components is currently not guarded by
dedicated authentication or encryption and is assumed to be confined to a
private network.

## How to build `wire-server`

There are two options:

#### 1. Compile sources natively. 

This requires a range of dependencies that depend on your platform/OS, such as:

- Haskell compiler and package manager: GHC, stack 
- Rust compiler and package manager: rustc, cargo
- some development packages (libsodium, openssl, protobuf, icu, geoip, snappy, ...) that depend on your platform/OS
- installed [cryptobox-c](https://github.com/wireapp/cryptobox-c)
    
See for instance the complete setup for 

- alpine: [setup for Haskell services](build/alpine/Dockerfile.builder), [setup for nginz](services/nginz/Dockerfile)
- ubuntu: (TODO)

Once all dependencies are set up, the following should succeed:

```bash
# build haskell services
make
# build nginz
cd services/nginz && make
```

#### 2. Use docker

Ready-built images can be downloaded from [here](https://hub.docker.com/r/wireserver/).

If you wish to build your own docker images, you need [docker version >= 17.05](https://www.docker.com/) and [`make`](https://www.gnu.org/software/make/). Then,

```bash
make docker-services
```

will, eventually, after a few hours, have built a range of docker images. See the various Makefiles and docker images for details.

## How to run integration tests

TODO

## How to run `wire-server`

### External dependencies

 * Amazon account with access to
  * SES
  * SQS
  * SNS
  * S3
  * Cloudfront
  * DynamoDB
 * Nexmo/Twilio accounts (if you want to send out SMSes)

TODO: ....

## Roadmap

- Documentation on development
- Build and deployment options
