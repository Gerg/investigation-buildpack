# Investigation buildpack for Cloud Foundry

This buildpack provides a mechanism to inspect the runtime environment of an
app staging task on Cloud Foundry. It allows invocation of a custom command in
the compile step and then waiting for a configurable period of time. The
`detect` script returns 1, so the buildpack is never selected automatically.

## Usage

Use as a Git-based buildpack:

```
$ cf push my-app -b https://github.com/ematpl/investigation-buildpack.git
```

Install the buildpack in your Cloud Foundry deployment (requires admin access):

```
$ git clone https://github.com/ematpl/investigation-buildpack.git
$ cf create-buildpack investigation investigation-buildpack 1000
```

Custom configuration:

```
$ cf push my-app -b investigation --no-start
$ cf set-env my-app INV_COMMAND 'echo "$VCAP_APPLICATION"'
$ cf set-env my-app INV_WAIT_DURATION_SECONDS 300
$ cf start my-app
```

## Configuration

The buildpack respects the following environment variables to provide custom
functionality:

- `INV_COMMAND`: Command to execute at the start of the compilation step. No
  default.
- `INV_WAIT`: Whether to wait for some period of time after invoking the
  compile script. Defaults to `true`.
- `INV_WAIT_DURATION_SECONDS`: Approximate duration in seconds to wait: will be
  rounded down to the nearest 5 seconds. Defaults to `900`.

