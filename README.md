# Basic CRUD Policies

## Policies


A basic resource, called `basicResource` has been defined with `create`, `read`, `update` and `delete` actions. This policy expects a principal to have either the `ADMIN` or `USER` role.


A `basicResource` is expected to have two attributes, `ownerId` and `isPublished` which are used in the policy to make decisions with.


### Attribute Schema
Optional [attribute schema](https://docs.cerbos.dev/cerbos/latest/policies/schemas.html) has been defined for both the principal and the `basicResource` policy. These are JSONSchema definitions that are used by the Cerbos PDP at request time to validate the incoming request having all the required data to make a correct authorization decision from. The server configuration can be set to either give a warning or reject the request fully if the input doesn't conform to these schemas.


## Tests

The `/tests` directory contains a [single test suite](https://docs.cerbos.dev/cerbos/latest/policies/compile.html#testing) and related test data in order to check that the permissions are implemented as expected.


## Cerbos Cloud
This repo contains the `.cerbos-cloud.yaml` file in the root which is used by [Cerbos Cloud](https://cerbos.dev/cloud) to determine which commits to bundle and deploy to any connected Cerbos PDP instances:


```yaml
---
apiVersion: cloudapi.cerbos.dev/v1
labels: # List of bundle labels to be built when the git reference matches
 latest:
   branch: main
```


## Running Locally


The simplest way to run Cerbos is using the [container](https://docs.cerbos.dev/cerbos/latest/installation/container.html) which is shown below. Other options include running the binary avaliable [on the documentation site](https://docs.cerbos.dev/cerbos/latest/installation/binary.html).


### Compile & Test
```
docker run -i -t \
 -v $(pwd):/basic-crud \
 ghcr.io/cerbos/cerbos:latest compile --tests=/basic-crud/tests /basic-crud/policies
```


### Server
```
docker run --rm --name cerbos \
 -v $(pwd):/basic-crud \
 -p 3592:3592 \
 -p 3593:3593 \
 ghcr.io/cerbos/cerbos:latest server --config=/basic-crud/config.yaml
```
This will start a PDP locally configured via the `/config.yaml` file. The API documentation can be found at [`http://localhost:3592`
](http://localhost:3592)