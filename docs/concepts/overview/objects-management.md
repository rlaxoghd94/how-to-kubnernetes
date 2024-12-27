# Kubernetes Object Management

The `kubectl` CLI tool supports several different ways to create and manage Kubernetes objects.

## Management techniques

> **Warning**: A Kubernetes object should be managed using only one technique. **Mixing and matching techniques for the same object results in undefined behaviour**.

| Management techniques            | Operates on          | Recommended environment | Supported writers | Learning curve |
| -------------------------------- | -------------------- | ----------------------- | ----------------- | -------------- |
| Imperative commands              | Live objects         | Development projects    | 1+                | Lowest         |
| Imperative object configuration  | Individual files     | Production projects     | 1                 | Moderate       |
| Declarative object configuration | Directories of files | Production projects     | 1+                | Highest        |

## Imperative commands

With imperative commands, a user operates directly on live objects in a cluster. The user provides operations to the `kubectl` command as arguments or flags.

This is the recommended way to get started or to run a one-off task in a cluster. Because this technique operates directly on live objects, it provides no history of previous configurations.

### Examples

Run an instance of the nginx container by creating a Deployment object:

```bash
kubectl create deployment nginx --image nginx
```

### Trade-offs

Advantages compare to object configuration:

- Commands are expressed as a single action word
- Commands require only a single step to make changes to the cluster

Disadvantages compared to object configuration:

- Commands do not integrate with change review processes
- Commands do not provide an audit trail associated with changes
- Commands do not provide a source of records except for what is live
- Commands do not provide template for creating new objects

## Imperative object configuration

In imperative configuration, the `kubectl` command specifies the operation(create, replace, etc.), optional flags and at least one file name. The file specified must contain a full definition of the object in YAML or JSON format.

> **Warning**: The imperative `replace` command replaces the existing spec with the newly provided one, dropping all changes to the object missing from the configuration file. This approach should not be used with resource types whose specs are updated independently of the configuration file. Service of type `LoadBalancer`, for example, have their `externalIPs` field updated independently from the configuration by the cluster.


### Examples

Create the objects defined in a configuration file:

```bash
kubectl create -f nginx.yaml
```

Delete the objects defined in two configuration files:

```bash
kubectl delete -f nginx.yaml -f redis.yaml
```

Update the objects defined in a configuration file by overwriting the live configuration:

```bash
kubectl replace -f nginx.yaml
```

### Trade-offs

Advantages compared to *imperative commands*:

- Object configuration can be stored in a source control system such as GIT
- Object configuration can integrate with processes such as reviewing changes before push and audit trails
- Object configuration provides a template for creating new objects

Disadvantages compared to *imperative commands*:

- Object configuration requires basic understanding of the object schema
- Object configuration required the additional step of writing a YAML file

Advantages compared to *declarative object configuration*:

- Imperative object configuration behaviour is simpler and easier to understand
- As of Kubernetes v1.5, imperative object configuration is more mature

Disadvantages compare to *declarative object configuration*:

- Imperative object configuration works best on files, not directories
- Updates to live object must be reflected in configuration files, or they will be lost during the next replacement

## Declarative object configuration

When using declarative object configuration, a user operates on object configuration files store locally, however th user does not define the operations to be taken on the files. Create, update, and delete operations are automatically detected per-object by `kubectl`. This enables working on directories, where different operations might be needed for different objects.

> **Note**: Declarative object configuration retains changes made by other writers, even if the changes are not merged back to the object configuration file. This is possible by using the `patch` API operation to write only observed differences, instead of using the `replace` API operation to replace the entire object configuration.
>

### Examples

Process all object configuration files in the `configs` directory, and create or patch the live objects. You can first `diff` to see what changes are going to be made, and then apply:

```bash
kubectl diff -f configs/
kubectl apply -f configs/
```

Recursively process directories:

```bash
kubectl diff -R -f configs/
kubectl apply -R -f configs/
```

### Trade-offs

Advantages compare to *imperative object configuration*:

- Changes made directly to live objects are retained, even if they are not merged back into the configuration files
- Declarative object configuration has better support for operating on directories and automatically detecting operation types (create, patch, delete) per-object

Disadvantages compared to *imperative object configuration*:

- Declarative object configuration is harder to debug and understand results when they are unexpected
- Partial updates using `diff`s create complex merge and patch operations
