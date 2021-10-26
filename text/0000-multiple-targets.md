# Meta
[meta]: #meta
- Name: Support multiple targets/images in project descriptor
- Start Date: 2021-10-26
- Author(s): tbd
- Status: Draft <!-- Acceptable values: Draft, Approved, On Hold, Superseded -->
- RFC Pull Request: (leave blank)
- CNB Pull Request: (leave blank)
- CNB Issue: (leave blank)
- Supersedes: N/A

# Summary
[summary]: #summary

Add a new `targets`/`images` key in the `project.toml` file.

# Definitions
[definitions]: #definitions

* _project descriptor_ - The `project.toml` file that describes a project.

# Motivation
[motivation]: #motivation

Currently there is no support for specifying the output image in the `project.toml` although flags like `--tags` are already present.

# What it is
[what-it-is]: #what-it-is

This provides a high level overview of the feature.

- Define any new terminology.
- Define the target persona: buildpack author, buildpack user, platform operator, platform implementor, and/or project contributor.
- Explaining the feature largely in terms of examples.
- If applicable, provide sample error messages, deprecation warnings, or migration guidance.
- If applicable, describe the differences between teaching this to existing users and new users.

# How it Works
[how-it-works]: #how-it-works

* Accepted project descriptor related RFCs
  * https://github.com/buildpacks/rfcs/blob/main/text/0019-project-descriptor.md
  * https://github.com/buildpacks/rfcs/blob/main/text/0054-project-descriptor-schema.md
  * https://github.com/buildpacks/rfcs/blob/main/text/0080-builder-key-in-project-descriptor.md (could serve as a role-model)
  * https://github.com/buildpacks/rfcs/blob/main/text/0084-project-descriptor-domains.md
  * Closed PR which includes some similar changes: https://github.com/buildpacks/rfcs/blob/project-descriptor-simple/text/0000-project-descriptor.md

First proposal:

```toml
[[targets]]
name = "spring-petclinic"
registry = "gcr.io"
tags = ["latest", "v1"]

[[targets]]
name = "spring-petclinic"
registry = "hub.docker.com"
tags = ["latest", "v1"]
```

* Tags:
    * Could be read from the `--tags` flag
    * Could be defaulted to `project.lversion` field of `project.toml`
* Name: Could just be taken from `project.name` (if present)

Alternatives:
```toml
[targets]
registries = ["gcr.io", "hub.docker.com"]
name = "spring-petclinic"
tags = ["latest", "v1"]
```

```toml
[[targets]]
registry = "gcr.io"
name = "spring-petclinic"
tag = "latest"

[[targets]]
registry = "hub.docker.com"
name = "spring-petclinic"
tag = "v1"

# ...
```

* Simple array of images containing all information i.e. `registry`, `name`, `tag`

```toml
targets = [
    "gcr.io/spring-petclinic:latest",
    "gcr.io/spring-petclinic:v1",
    "hub.docker.com/spring-petclinic:latest",
    "hub.docker.com/spring-petclinic:v1"
]
```

* Simple array of images containing all information with `name` being a placeholder
  * Beneficial for usage with staging service

```toml
name = "spring-petclinic"
targets = [
    "gcr.io/{name}:latest",
    "gcr.io/{name}:v1",
    "hub.docker.com/{name}:latest",
    "hub.docker.com/{name}:v1"
]
```

# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

# Alternatives
[alternatives]: #alternatives

- What other designs have been considered?
- Why is this proposal the best?
- What is the impact of not doing this?

# Prior Art
[prior-art]: #prior-art

Discuss prior art, both the good and bad.

# Unresolved Questions
[unresolved-questions]: #unresolved-questions

- What parts of the design do you expect to be resolved before this gets merged?
- What parts of the design do you expect to be resolved through implementation of the feature?
- What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?

# Spec. Changes (OPTIONAL)
[spec-changes]: #spec-changes
Does this RFC entail any proposed changes to the core specifications or extensions? If so, please document changes here.
Examples of a spec. change might be new lifecycle flags, new `buildpack.toml` fields, new fields in the buildpackage label, etc.
This section is not intended to be binding, but as discussion of an RFC unfolds, if spec changes are necessary, they should be documented here.
