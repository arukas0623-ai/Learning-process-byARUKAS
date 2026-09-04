# Ability Map Specification

Version: `0.2.0-alpha`

An Ability Map Provider describes what a learner needs to be able to do. It
does not assert what the learner already knows.

## Required node shape

Every provider must expose stable ability nodes with:

```yaml
id: stable-node-id
name: Human-readable ability name
description: Observable capability boundary
prerequisites: []
assessment_methods: [recall, explanation, application, transfer]
```

Providers may add domain-specific metadata, but they must preserve these
fields and stable IDs across exports. Nodes should identify source references
when they were extracted from a course, book, document or project.

APKM is one provider implementation used by the personal example; it is not a
Core dependency.
