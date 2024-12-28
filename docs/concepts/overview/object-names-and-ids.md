# Object Names and IDs

Each object in your cluster has a name that is unique for that type of resource. Every Kubernetes object also has a UID tha tis unique across your whole cluster.

## Names

A client-provided string that refers to an object in a resource URL, such as `/api/v1/pods/some-name`.

Only one object of a given kind can have a given name at a time. However, if you delete the object, you can make a new object with the same name.

**Names must be unique across all API versions of the same resource. API resources are distinguished by their API group, resource type, namespace(for namespaced resources), and name. In other words, API version is irrelevant in this context.

> **Note**: In cases when objects represent a physical entity, like a Node representing a physical host, when the host is re-created under the same name without deleting and re-creating the Node, Kubernetes treats the new host as the old one, which may lead to inconsistencies.

The server may generate a name when `generateName` is provided instead of `name` in a resource create request. When `generateName` is used, the provided value is used as a name prefix, which server appends a generated suffix to. Even though the name is generated, it may conflict with the existing names resulting in a HTTP 409(Conflict) response.

Below are four types of commonly used name constraints for resources.

### DNS Subdomain Names

Most resource types require a name that can be used as a DNS subdomain name as defined in RFC 1123. This means the name must:

- contain no more than 253 characters
- contain only lowercase alphanumeric characters, "_" or "."
- start with an alphanumeric character
- end with an alphanumeric character

### Path Segment Names

Some resource types require their names to be able to be safely encoded as a path segment. In other words, the name may not be "." or ".." and the name may not contain "/" or "%".

Example: `nginx-demo` named Pod manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-demo
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

### UIDs

A Kubernetes systems-generated string to uniquely identify objects.

Every object created over the whole lifetime of a Kubernetes cluster has a distinct UID. It is intended to distinguish between historical occurrences of similar entities.

Kubernetes UIDs are universally unique identifiers, also known as UUIDs.