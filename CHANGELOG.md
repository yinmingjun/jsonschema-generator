# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- Support for custom definitions in the scope of a particular field/method via `SchemaGeneratorConfigPart.withCustomDefinitionProvider()`
- Ability to opt-out of normal "attribute collection" for custom definitions through new `CustomDefinition` constructor parameters

## [4.7.0] - 2020-03-20
### Added
- Support for multiple target type overrides at once per field/method
- Support for "$id" property via `SchemaGeneratorGeneralConfigPart.withIdResolver()`
- Support for "$anchor" property via `SchemaGeneratorGeneralConfigPart.withAnchorResolver()`

### Fixed
- Allow for multiple types with the same name (but different package) instead of picking one at random
- Allow for multiple definitions for the same type (e.g. in case of a custom definition wrapping the standard definition)

### Deprecated
- Configuration option for single target type override
- Look-up of single target type override from configuration

## [4.6.0] - 2020-03-15
### Added
- Explicit indication of targeted JSON Schema version (for now: Draft 7 or 2019-09)
- Support for renamed keywords between Draft versions through new `SchemaKeyword` enum (replacing static `SchemaConstants` interface)
- Support for `$ref` alongside other attributes (from Draft 2019-09 onwards) thereby reducing number of `allOf` elements
- Introduce `Option.ALLOF_CLEANUP_AT_THE_END` (also included in all standard `OptionPreset`s) for toggling-off this improvement if not desired

### Changed
- Reduce number of `allOf` elements in generated schema when contained schema parts are distinct (and in case of Draft 7: don't contain `$ref`)

### Deprecated
- `SchemaConstants` interface containing static `String` constants for tag names/values (in favour of version-aware `SchemaKeyword` enum)
- Internal `AttributeCollector` API without generation context as parameter
- `SchemaGeneratorConfigBuilder` constructors without explicit JSON Schema version indication

### Dependency Update
- `com.fasterxml.jackson.core`:`jackson-core`/`jackson-databind` from `2.10.0` to `2.10.2`
- `com.fasterxml`:`classmate` from `1.5.0` to `1.5.2`
- Optional: `org.slf4j`:`slf4j-api` from `1.7.26` to `1.7.30`
- Optional: `org.apache.logging.log4j`:`log4j-api`/`log4j-core`/`log4j-slf4j-impl` from `2.11.2` to `2.13.0`

## [4.5.0] - 2020-03-05
### Added
- Expose `SchemaGenerationContext.createDefinitionReference()` to custom definitions to enable circular references within
- Expose `SchemaGenerationContext.createStandardDefinitionReference()` to custom definitions to enable circular references within

## [4.4.0] - 2020-03-02
### Added
- Enable declaration of subtypes through `withSubtypeResolver(SubtypeResolver)` on `forTypesInGeneral()` (#24)

### Changed
- Move custom definitions and type attribute overrides into `forTypesInGeneral()` (while preserving delegate setters on config builder)

## [4.3.0] - 2020-02-28
### Changed
- Limit collected type attributes by declared "type" (prior to any `TypeAttributeOverride`!)

### Fixed
- Not declare any "type" for `Object.class` by default

## [4.2.0] - 2020-02-27
### Added
- Support for "additionalProperties" property via `SchemaGeneratorTypeConfigPart.withAdditionalPropertiesResolver()`
- Support for "patternProperties" property via `SchemaGeneratorTypeConfigPart.withPatternPropertiesResolver()`
- Introduce new `Option.FORBIDDEN_ADDITIONAL_PROPERTIES_BY_DEFAULT` for more convenient usage
- Offer `TypeScope.getTypeParameterFor()` and `TypeContext.getTypeParameterFor()` convenience methods

### Fixed
- Possible exceptions in case of encountered collections without specific type parameters

## [4.1.0] - 2020-02-18
### Added
- New `Option.FLATTENED_ENUMS_FROM_TOSTRING`, using `toString()` instead of `name()` as per `Option.FLATTENED_ENUMS`

## [4.0.2] - 2020-01-30
### Fixed
- Avoid further characters in definition keys that are not URI-compatible (#19)

## [4.0.1] - 2020-01-29
### Fixed
- Avoid white-spaces in definition keys

## [4.0.0] - 2020-01-03
### Added
- Extended API for defining context independent collection of schema attributes (for array/collection items that are not directly under a member)

### Changed
- BREAKING CHANGE: `TypeAttributeOverride`'s second parameter is now the new `TypeScope` wrapper instead of just a `ResolvedType`

## [3.5.0] - 2020-01-01
### Added
- `CustomDefinitionProviderV2` with access to `SchemaGenerationContext`, e.g. to allow continuing normal schema generation for nested properties

### Deprecated
- `CustomDefinitionProvider` receiving only the `TypeContext` as parameter

### Fixed
- Possible `IllegalAccess` when loading constant values should just be ignored

## [3.4.1] - 2019-12-30
### Fixed
- Collected attributes should also be applied to container types (Issue #15)

## [3.4.0] - 2019-11-29
### Added
- Introduce convenience function `MemberScope.getAnnotationConsideringFieldAndGetter(Class)`

## [3.3.0] - 2019-10-25
### Fixed
- Increase dependency version for jackson-databind (and jackson-core) to resolve security alerts.
- Avoid unnecessary quotes when representing constant string values (due to changed behaviour in jackson >=2.10.0)

## [3.2.0] – 2019-09-01
### Added
- In `SchemaGenerator.generateSchema(Type)` also allow passing type parameters via `SchemaGenerator.generateSchema(Type, Type...)`.
- Support for "required" property via `SchemaGeneratorConfigPart.withRequiredCheck()` (PR #5).
- Support for "default" property via `SchemaGeneratorConfigPart.withDefaultResolver()` (PR #5).
- Support for "pattern" property via `SchemaGeneratorConfigPart.withStringPatternResolver()` (Issue #9).

## [3.1.0] – 2019-06-18
### Changed
- Use `Class.getTypeName()` instead of `Class.getName()` in `TypeContext.getFullTypeDescription()`.

### Added
- In `TypeContext.resolve(Type)` also allow passing type parameters via `TypeContext.resolve(Type, Type...)`.

### Fixed
- `NullPointerException` on `MemberScope` representing `void` methods.
- `IndexOutOfBoundsException` when determining container item type of raw `Collection`.

## [3.0.0] – 2019-06-10
### Changed
- Simplify configuration API to be based on `FieldScope`/`MethodScope` respectively.
- Consolidate some utility functions into `FieldScope`/`MethodScope`.
- Consolidate logic for determining whether a type is a container into `TypeContext`.
- Consolidate naming logic for schema "definitions" into `TypeContext`.
- Add `TypeContext` argument to `CustomDefinitionProvider` interface.
- Remove `SchemaGeneratorConfig` argument from `InstanceAttributeOverride` interface.

### Added
- Allow for sub-typing of `FieldScope`/`MethodScope` and `TypeContext` (in case offered configuration options are insufficient).

### Removed
- Remove support for `Option.GETTER_ATTRIBUTES_FOR_FIELDS` and `Option.FIELD_ATTRIBUTES_FOR_GETTERS`, rather let configurations/modules decide individually and avoid potential endless loop.

## [2.0.0] – 2019-06-07
### Changed
- Removed type resolution and replaced it with `org.fasterxml:classmate` dependency.
- Adjusting configuration API to use `classmate` references for types/fields/methods.

### Fixed
- Ignore complex constant values that may not be properly representable as JSON.

## [1.0.2] - 2019-05-30
### Fixed
- Increase dependency version for jackson-databind to resolve security alert.

## [1.0.1] - 2019-05-19
### Fixed
- Specified "test" scope for dependency on jsonassert.

## [1.0.0] - 2019-05-18
### Added
- Reflection-based JSON Schema Generator.
- Support of generics and the resolution of their type boundaries in the respective scope.
- Inclusion of any fields and public methods in a generated JSON Schema.
- Ability to define a fixed JSON Schema per type (e.g. for determining how to represent primitive types).
- Ability to customise various aspects of schema generation and assigned attributes.
- Concept of modules (e.g. for sub-libraries) to define a single entry-point for applying individual configurations.
- Standard enum of common configuration options.
- Specific handling of enums (two alternatives via standard options).
- Specific handling of optionals (two alternatives via standard options).
- Pre-defined sets of standard options to cover different use-cases and simplify library usage.


[Unreleased]: https://github.com/victools/jsonschema-generator/compare/v4.7.0...HEAD
[4.7.0]: https://github.com/victools/jsonschema-generator/compare/v4.6.0...v4.7.0
[4.6.0]: https://github.com/victools/jsonschema-generator/compare/v4.5.0...v4.6.0
[4.5.0]: https://github.com/victools/jsonschema-generator/compare/v4.4.0...v4.5.0
[4.4.0]: https://github.com/victools/jsonschema-generator/compare/v4.3.0...v4.4.0
[4.3.0]: https://github.com/victools/jsonschema-generator/compare/v4.2.0...v4.3.0
[4.2.0]: https://github.com/victools/jsonschema-generator/compare/v4.1.0...v4.2.0
[4.1.0]: https://github.com/victools/jsonschema-generator/compare/v4.0.2...v4.1.0
[4.0.2]: https://github.com/victools/jsonschema-generator/compare/v4.0.1...v4.0.2
[4.0.1]: https://github.com/victools/jsonschema-generator/compare/v4.0.0...v4.0.1
[4.0.0]: https://github.com/victools/jsonschema-generator/compare/v3.5.0...v4.0.0
[3.5.0]: https://github.com/victools/jsonschema-generator/compare/v3.4.1...v3.5.0
[3.4.1]: https://github.com/victools/jsonschema-generator/compare/v3.4.0...v3.4.1
[3.4.0]: https://github.com/victools/jsonschema-generator/compare/v3.3.0...v3.4.0
[3.3.0]: https://github.com/victools/jsonschema-generator/compare/v3.2.0...v3.3.0
[3.2.0]: https://github.com/victools/jsonschema-generator/compare/v3.1.0...v3.2.0
[3.1.0]: https://github.com/victools/jsonschema-generator/compare/v3.0.0...v3.1.0
[3.0.0]: https://github.com/victools/jsonschema-generator/compare/v2.0.0...v3.0.0
[2.0.0]: https://github.com/victools/jsonschema-generator/compare/v1.0.2...v2.0.0
[1.0.2]: https://github.com/victools/jsonschema-generator/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/victools/jsonschema-generator/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/victools/jsonschema-generator/releases/tag/v1.0.0
