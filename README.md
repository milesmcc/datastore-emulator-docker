# Google Cloud Emulator

A GCP Emulator. Supports the [Google Cloud Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator/) and the [Google Cloud PubSub Emulator](https://cloud.google.com/pubsub/docs/emulator). The image is meant to be used for creating an standalone emulator for testing.

## Environment

The following environment variables must be set:

Datastore:

- `DATASTORE_LISTEN_ADDRESS`: The address should refer to a listen address, meaning that `0.0.0.0` can be used. The address must use the syntax `HOST:PORT`, for example `0.0.0.0:8081`. The container must expose the port used by the Datastore emulator.
- `DATASTORE_PROJECT_ID`: The ID of the Google Cloud project for the emulator.

PubSub:

The following environment variables must be set:

- `PUBSUB_LISTEN_ADDRESS`: The address should refer to a listen address, meaning that 0.0.0.0 can be used. The address must use the syntax HOST:PORT, for example 0.0.0.0:8432. The container must expose the port used by the Pub/Sub emulator.
- `PUBSUB_PROJECT_ID`: The ID of the Google Cloud project for the emulator.

## Custom commands

This image contains a script named `start-datastore` and `start-pubsub` (included in the PATH). This script is used to initialize the Datastore emulator and PubSub emulator, respectively.

### Starting an emulator

By default, the following command is called:

```sh
start-datastore
```

To use the PubSub emulator instead, change the entrypoint to `start-pubsub`.

### Starting an emulator with options (Datastore)

This image comes with the following options: `--no-store-on-disk` and `--consistency`. Check [Datastore Emulator Start](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/datastore/start). `--legacy`, `--data-dir` and `--host-port` are not supported by this image.

```sh
start-datastore --no-store-on-disk --consistency=1.0
```

## Creating a Datastore emulator with Docker Compose

The easiest way to create an emulator with this image is by using [Docker Compose](https://docs.docker.com/compose). The following snippet can be used as a `docker-compose.yml` for a datastore emulator:

```YAML
version: "2"

services:
  datastore:
    image: milesmcc/gcp-emulator
    environment:
      - DATASTORE_PROJECT_ID=project-test
      - DATASTORE_LISTEN_ADDRESS=0.0.0.0:8081
    ports:
      - "8081:8081"
```

### Persistence

The image has a volume mounted at `/opt/data`. To maintain states between restarts, mount a volume at this location.
