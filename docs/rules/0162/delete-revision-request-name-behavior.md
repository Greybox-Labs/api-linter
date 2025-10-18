---
rule:
  aep: 162
  name: [core, '0162', delete-revision-request-name-behavior]
  summary: |
    Delete Revision requests should annotate the `name` field with `aep.api.field_behavior`.
permalink: /162/delete-revision-request-name-behavior
redirect_from:
  - /0162/delete-revision-request-name-behavior
---

# Delete Revision requests: Name field behavior

This rule enforces that all Delete Revision requests have
`aep.api.field_behavior` set to `FIELD_BEHAVIOR_REQUIRED` on their `string name` field, as
mandated in [AEP-162][].

## Details

This rule looks at any message matching `Delete*RevisionRequest` and complains if the
`name` field does not have a `aep.api.field_behavior` annotation with a
value of `FIELD_BEHAVIOR_REQUIRED`.

## Examples

**Incorrect** code for this rule:

```proto
// Incorrect.
message DeleteBookRevisionRequest {
  // The `aep.api.field_behavior` annotation should also be included.
  string name = 1 [
    (aep.api.field_info).resource_reference.type = "library.googleapis.com/Book"
  ];
}
```

**Correct** code for this rule:

```proto
// Correct.
message DeleteBookRevisionRequest {
  string name = 1 [
    (aep.api.field_behavior) = FIELD_BEHAVIOR_REQUIRED,
    (aep.api.field_info).resource_reference.type = "library.googleapis.com/Book"
  ];
}
```

## Disabling

If you need to violate this rule, use a leading comment above the field.
Remember to also include an [aep.dev/not-precedent][] comment explaining why.

```proto
message DeleteBookRevisionRequest {
  // (-- api-linter: core::0162::delete-revision-request-name-behavior=disabled
  //     aep.dev/not-precedent: We need to do this because reasons. --)
  string name = 1 [
    (aep.api.field_info).resource_reference.type = "library.googleapis.com/Book"
  ];
}
```

If you need to violate this rule for an entire file, place the comment at the
top of the file.

[aep-162]: https://aep.dev/162
[aep.dev/not-precedent]: https://aep.dev/not-precedent
