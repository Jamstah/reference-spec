# OCI Reference Specification

This specification covers OCI registry compliant container image references.

A reference looks like:
```
[<registry>/]<name>[:tag][@digest]
```

References may be used for multiple purposes. These are:
- Inspect
- Push
- Pull
- List tags
- Delete

A fully specified reference includes the registry and at least a tag or digest.

A reference can refer to a repository or a specific image. To refer to a specific image it must include either a tag or a digest.

TODO: BNF? References to URI spec?

TODO: Is it valid to include both a tag and a digest?

## Names

The repository name **MUST** match the following regular expression:

```
[a-z0-9]+((\.|_|__|-+)[a-z0-9]+)*(/[a-z0-9]+((\.|_|__|-+)[a-z0-9]+)*)*
```

(https://github.com/opencontainers/distribution-spec/blob/main/spec.md#workflow-categories)

TODO: Note on including the path separator? It's treated as an opaque string for OCI spec purposes but clients can use it for managing image hierarchies.

## Tags

A tag MUST be at most 128 characters in length and **MUST** match the following regular expression:

```
[a-zA-Z0-9_][a-zA-Z0-9._-]{0,127}
```

(https://github.com/opencontainers/distribution-spec/blob/main/spec.md#workflow-categories)

TODO: There are open issues around including + in a tag specification for semver compatibility. Would that be saved for a v2, and we'd keep v1 to things that are already widely compatible and understood?

## Digests

A digest is of the form:
```
(?<algorithm>[a-zA-Z0-9]+):(?<digest>[a-zA-Z0-9]+)
```

## Detecting the registry

It's not easy to detect if the first path of the path is a registry... should we give advice on when to treat it as a hostname and when not to?

- If there is no `/` in the reference, then the registry host has not been specified.
- If there is a `/` in the reference, the string before the first `/` may be a registry host.
- If there is a `:` in the first segment then it is definitely a registry host
- What else could we suggest?

If there is no hostname, behaviour is down to the client.

## When a registry is not specified

Client choice - could be look up in local cache then attempt registry access, could be rejected...

## Canonicalization

Can we canonicalize with a tag or only a digest? What if there are both?

## Transports

Worth referencing different transports? Even if its just to say they exist and are client specific?

https://www.mankier.com/1/skopeo#Image_Names
