# Schema Compatibility

gstack uses JSON Schema definitions in `schemas/` for cross-runtime file formats
(dispatch, completion, handoff, activity). Both the gstack dispatch daemon and
OpenClaw bridge skill validate against these schemas.

## Version policy

Schema version is tied to gstack's `VERSION` file.

### Breaking change (major version bump required)

A change is **breaking** if it:
- Removes a required field
- Changes a field's type (e.g., string to integer)
- Changes the semantics of an existing field (e.g., `duration` from milliseconds to seconds)
- Narrows an enum (removes a valid value)

### Non-breaking change (minor version bump)

A change is **non-breaking** if it:
- Adds a new optional field
- Widens an enum (adds a new valid value)
- Relaxes a constraint (e.g., increases maxLength)
- Adds a new schema file

### Rules

1. **Additive by default.** New fields are always optional. Existing readers ignore unknown fields.
2. **Breaking changes require a major version bump** and must be called out in CHANGELOG.
3. **Both sides validate.** The gstack daemon validates incoming dispatch files. The OpenClaw bridge validates completion reports. If validation fails, the file is rejected with an error status, not silently ignored.
4. **Schema version in file.** Future versions may add a `schema_version` field to each file for explicit version negotiation. For now, schema version is implied by gstack VERSION.
