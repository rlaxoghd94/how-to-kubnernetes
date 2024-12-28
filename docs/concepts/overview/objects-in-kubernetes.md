# Objects in Kubernetes

## Understanding Kubernetes objects

*Kubernetes objects* are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster.

They can describe:

- What containerized applications are running (and on which nodes)
- The resources available to those applications
- The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance

A Kubernetes object is a "record of intent" -- once you create the object, the Kubernetes system will constantly work to ensure that the object exists. By creating an object, you're effectively telling the Kubernetes system what you want your cluster's workload to look like; this is your cluster's desired state.

To work with your Kubernetes objects -- whether to create, modify, or delete them -- you'll need to use the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/). When you use the `kubectl` command-line interface, for example, the CLI makes the necessary Kubernetes API calls for you.

### Object specs and status

All Kubernetes objects will contain a `spec` and a `status`.

- a `spec` is declared on creation time to describe a *desired state* of an object
- a `status` is the *current state* of the object, supplied and updated by the Kubernetes system and its components

The Kubernetes control plane continually and actively manages every object's actual state to match the desired state you supplied.

> **Example: "Deployment" in Kubernetes**
>
> In Kubernetes, a "Deployment" is an object that can represent an application running on your cluster. When you crate the Deployment, you might set the Deployment `spec` to specify that you want three replicas of the application to be running. The Kubernetes system reads the Deployment spec and starts three instances of your desired application -- updating the status to match your spec. If any of those instances should fail, a status change, the Kubernetes system responds to the difference between `spec` and `status` by making a correction -- in this case, starting a replacement instances.


### Describing a Kubernetes object

When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object, such as a name. When you use the Kubernetes API to create the object, either directly or via `kubectl`, that API request must include that information as JSON in the request body. Most often, you provide the information to `kubectl` in a file known as a ***manifest***. By convention, manifests are YAML(you could also use JSON format). Tools such as `kubectl` converts the information from a manifest into JSON or another supported serialization format when making the API request over HTTP.

The following is a sample manifest that shows the required fields and object spec for a Kubernetes Deployment:

```yaml
# application/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

One way to create a Deployment using a manifest file like the one above is to use the `kubectl apply` command in the `kubectl` CLI, passing the `.yaml` file as an argument.

```bash
kubectl apply -f https://k8s.io/examples/application/deployment.yaml

# Console Output
deployment.apps/nginx-deployment created
```

### Required fields

In the manifest provide above, you'll need to set values for the following fields:

- `apiVersion`: version of the Kubernetes API to create this object with
- `kind`: kind of object to create
- `metadata`: Data that helps uniquely identify the object, including a `name`, `UID`, and optional `namespace`
- `spec`: desired state for the object

The precise format of the object `spec` differs for every Kubernetes object, and contains nested fields specific to that object. Use the documents for the assist.

## Server-side field validation

Starting with Kubernetes v1.25, the API server offers server-side fields validation that detects unrecognized or duplicate fields in an object. It provides all the functionality of `kubectl --validate` on the server-side.

The `kubectl` tool uses the `--validate` flag to set the level of the field validation. It accepts the values `ignore`, `warn`, and `strict` while also accepting the value `true`(equivalent to `strict`) and `false`(equivalent to `ignore`). The default validation setting for `kubectl` is `--validate=true`.

**Strict**

Strict field validation, errors on validation failure

**Warn**

Field validation is performed, but errors are exposed as warnings rather than failing the request

**Ignore**

No server-side field validation is performed

When `kubectl` cannot connect to an API server that supports fields validation it will fall back to using client-side validation. Kubernetes 1.27+ versions always offer field validations; older releases might not.

