# Getting started with basic CRUD policies

## What's in this repository?

### Policies

[Resource policies](https://docs.cerbos.dev/cerbos/latest/resource_policies.html) define rules for actions that can be performed on a given resource.

[basicResource.yaml](./basicResource.yaml) defines a resource policy for `basicResource`s, with `create`, `read`, `update` and `delete` actions. This policy expects a principal to have either the `ADMIN` or `USER` role.

A `basicResource` is expected to have two attributes, `ownerId` and `isPublished`, which are used in the policy to make decisions about which actions should be permitted.

### Attribute schemas

[Attribute schemas](https://docs.cerbos.dev/cerbos/latest/schemas.html) are optional JSON schemas that are used by the Cerbos PDP at request time to validate the incoming request having all the required data to make a correct authorization decision.
The server configuration can be set to either give a warning or reject the request if the input doesn't conform to these schemas.

[\_schemas/principal.json](./_schemas/principal.json) defines a schema for the principals, while [\_schemas/basicResource.json](./_schemas/basicResource.json) defines a schema for the `basicResource`s.

### Tests

[basicResource_test.yaml](./basicResource_test.yaml) defines a [test suite](https://docs.cerbos.dev/cerbos/latest/compile.html#testing) and related test data that checks that the permissions are implemented as expected.

### Cerbos Policy Decision Point (PDP) configuration

[.cerbos.yaml](./.cerbos.yaml) is used to configure a Cerbos PDP server container to load the policies from disk.

### Cerbos Hub configuration

[.cerbos-hub.yaml](./.cerbos-hub.yaml) is used to configure a [Cerbos Hub](https://cerbos.dev/next) workspace to compile policy bundles from commits matching the configured labels, to be deployed to connected Cerbos PDP instances.

## Running locally

The simplest way to run Cerbos is using the [container](https://docs.cerbos.dev/cerbos/latest/installation/container.html), which is shown below.
[See the documentation for other ways to install and run Cerbos locally](https://docs.cerbos.dev/cerbos/latest/installation/binary.html).

### Compile and test

Verify that the policies are correct by running

```
docker run --rm -it \
  -v $(pwd):/basic-crud \
  ghcr.io/cerbos/cerbos:latest \
  compile --verbose /basic-crud
```

### PDP server

Launch a PDP server by running

```
docker run --rm --name cerbos \
 -v $(pwd):/basic-crud \
 -p 3592:3592 \
 -p 3593:3593 \
 ghcr.io/cerbos/cerbos:latest \
 server --config=/basic-crud/.cerbos.yaml
```

The API documentation can then be found at [http://localhost:3592](http://localhost:3592).
