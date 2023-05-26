# Schemas

Schemas will be defined in either plantuml or mermaid. 
The source will be in the same directory as the markdown file using the schema.

## Coding conventions: C#

The configuration used is C# 11 with .NET 7 

- Use tabs. Default tab size is 2 spaces.
- Use PascalCase for classes, methods, properties, etc.
- Use camelCase for variables, parameters, etc.
- Use snake_case for constants.
- Use English for identifiers, methods, parameters, etc.
- Use ALL_CAPS for enums.
- Use ALL_CAPS_WITH_UNDERSCORES for constants.
- Use `nameof` where possible.
- Use `var` where possible.
- Use braces for all control structures.
- Avoid `this` where possible.
- Use `string` instead of `String`.
- Use `int` instead of `Int32`.
- Use `long` instead of `Int64`.
- Use `double` instead of `Double`.
- Use `float` instead of `Single`.
- Use `decimal` instead of `Decimal`.
- Use `bool` instead of `Boolean`.
- Use `object` instead of `Object`.
- Treat warnings as errors!
- Consolidate nuget packages, if possible
- Avoid multiple versions of nuget packages
- Do not write unsafe or OS-specific code

Note: the goal is to migrate to .NET 8 after release, as .NET 8 is Long Team Support.
