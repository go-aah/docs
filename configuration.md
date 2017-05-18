Title: aah framework Configuration
Desc: The configuration syntax is used by aah framework is forge syntax very similar to HOCON syntax. Enhanced by the author of aah framework. This config syntax used across the aah framework.
Keywords: aah config, aah configuration, HOCON, forge, config syntax, configuration syntax, aah.conf, routes.conf, security.conf, aah.project
---
# aah framework Configuration

The configuration syntax is used by aah framework is `forge` developed by [@brettlangdon](https://github.com/brettlangdon") very similar to HOCON syntax. Enhanced by the author of aah framework [@jeevatkm](https://github.com/jeevatkm). Syntax applies to `aah.conf`, `routes.conf`, `security.conf`, `i18n` message files and `aah.project` file.

### Table of Contents

  * [Comments](#comments)
  * [Braces](#braces)
  * [Supported value types](#supported-value-types)
  * [Key-value separator](#key-value-separator)
  * [Includes](#includes)
  * [Environment Variables](#environment-variables)
  * [Substitutions/Reference](#substitutions-reference)

## Comments

  * The line starts with `#` considered as comment.
  * You can have same line comment after `;`. For e.g: ` identifier = "value"; # comment here`

## Braces

  * Root doesn't start with curly braces.
  * Section must have curly braces - values are enclosed with opening and closing brace.  

## Supported value types

  * `String` - must be quoted string
  * `Integer` - positive and negative value supported
  * `Float` - positive and negative value supported
  * `Boolean` - true or false of any case (e.g. TRUE, True, true, FALSE, False, false)
  * `Null` - null
  * `List/Array` - string, integer, float supported. Values are separated by commas and surrounded by brackets. It can be multi line too

## Key-value and separator

  * The `=` character used to separate keys from values.
  * The key can have `underscore` in it.

## Includes

An include statement tells the config parser to include the contents of another config file where the include statement is defined. Includes are in the format `include "<pattern>"`. The `<pattern>` can be any glob like pattern which is compatible with [path.filepath.Match](http://golang.org/pkg/path/filepath/#Match). It can be absolute path or relative path to `config` directory.

## Environment Variable

An environment is a way to pull in environment variables into your config. Environment variables are identifiers which start with a dollar sign (For example: `$PATH`). Environment variables are always represented as strings and are evaluated at parse time. Adapted to supported value type.

## Substitutions/Reference

Substitutions/Reference are a way of referring to other parts of the configuration into another identifier.

  * **Global Reference** - An identifier which may contain periods, the references are resolved from the global section. For example: `global_value`, `section.sub_section.value`.
  * **Local Reference** - An identifier which main contain periods which starts with a period, the references are resolved from the settings current section. For example: `.value`, `.sub_section.value`.
