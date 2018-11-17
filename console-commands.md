Title: Console Commands
Desc: Console module provides capability to add user-defined commands easily into aah application binary.
Keywords: console, command, commands, app binary, cli
---
# aah Console Commands

This document provides the insight into aah Console module  `aahframe.work/console` usage. Introduced in <span class="badge lb-sm">v0.12.0</span>.

### Table of Contents

  * [Overview](#overview)
  * [Supported Flag Types & Accessing Value](#supported-flag-types-accessing-value)
  * [Creating User-Defined Command](#creating-user-defined-command)
  * [Creating Sub-Commands](#creating-sub-commands)
  * [Adding User-Defined Command](#adding-user-defined-commands)
  * [Customizing `version` and `help` Flag Description](#customizing-version-and-help-flag-description)
  * [Running console command using aah CLI](#running-console-command-using-aah-cli)

## Overview

In <span class="badge lb-sm">v0.12.0</span> release, aah introduced the console commands feature. Ability to add **`N`** no. of user-defined behavior(s) as a console command into aah application binary easily. 

By default application binary has with few important reserved commands - 

  * Command `run` - Runs application server 
  * Command `vfs` - Provides access to app VFS instance to interact with it
  * Command `help` - Shows a list of commands or help for one command

Console module `aahframe.work/console` is a type aliased package using `github.com/urfave/cli`. Important thing in this design approach is to keep decouple from third-party library also have ability introduce in-home library (if need be) with retaining all capabilities. From aah user prespective everything is smooth.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>aah application binary does few things before it gives control over to user-defind console command.
<ul>
<li>aah VFS initialized - <code>aah.App().VFS()</code> available</li>
<li>aah.conf initialized - <code>aah.App().Config()</code> available</li>
<li>aah application log initialized - <code>aah.App().Log()</code> available</li>
</p>
</div>

## Supported Flag Types & Accessing Value

Console command supports following flag types -

  * `console.StringFlag`
  * `console.BoolFlag`
  * `console.IntFlag`
  * `console.Int64Flag`
  * `console.Float64Flag`
  * `console.IntSlice`
  * `console.StringSlice`

### Example

```go
// Defining flag
console.StringFlag{
    Name:  "envprofile, e",     // long and short posix flag name
    Value: "dev",               // default flag value
    Usage: "Environment profile name to activate (e.g: dev, qa, prod)",
}

// Accessing flag value with action function
ctx.String("envprofile") // or ctx.String("e")
```

## Creating User-Defined Command

Creating user-defined command is very easy. Let's say we have directory/package under `<app-base-dir>/app/commands`-

```go
// User-defind `sample` command
var Sample = console.Command{
	Name:    "sample",
	Aliases: []string{"s"},
	Usage:   "This is sample user-defined command for aah application",
	Description: `This is long multi-line description about sample command

  Example:
    <app-binary> sample --message "hello sample command"
  `,
	Flags: []console.Flag{
		console.StringFlag{
			Name:  "message, m", // long and short flag name
			Usage: "Greeting message to sample command",
		},
	},
	Action: func(ctx *console.Context) error {
		fmt.Println("Welcome to sample command")
		fmt.Println("Greetings:", ctx.String("message"))
		return nil
	},
}
```

## Creating Sub-Commands

Creating sub commands are very easy. Let's say we have directory/package under `<app-base-dir>/app/commands`-

```go
// Sample Command with sub-command 'greet'
var Sample = console.Command{
	Name:    "sample",
	Aliases: []string{"s"},
	Usage:   "This is sample user-defined command for aah application",
	Description: `This is long multi-line description about sample command

  Example:
    <app-binary> sample <sub-command>
  `,
	Subcommands: []console.Command{
		{
			Name:    "greet",
			Aliases: []string{"g"},
			Usage:   "This is greet sub command for sample command",
			Description: `This long multi-line description about greet sub-command.
		
      Example:
        <app-binary> s g -m "hello greet sub-command"
			  <app-binary> sample greet --message "hello greet sub-command"
		  `,
			Flags: []console.Flag{
				console.StringFlag{
					Name:  "message, m", // long and short flag name
					Usage: "Greeting message",
				},
			},
			Action: func(ctx *console.Context) error {
				fmt.Println("Welcome to greet sub-command")
				fmt.Println("Greetings:", ctx.String("message"))
				return nil
			},
		},
	},
}
```

## Adding User-Defined Commands

Adding user-defined commands into aah application binary.

```go
func init() {
  app := aah.App()
  if err := app.AddCommand(Command1, Command2, Command3); err != nil {
    app.Log().Error(err)
  }
}
```

## Customizing `version` and `help` Flag Description

```go
func init() {
  console.VersionFlagDesc("your custom version flag description")
  console.HelpFlagDesc("your custom help flag description")
}

// Sample Output:
// --------------
// Global Options:
//   --help, -h     your custom help flag description
//   --version, -v  your custom version flag description
```

## Running console command using aah CLI

Typically aah user could run console command by building aah application and executing application binary.

It just aah CLI provides handy command that does it for you.

```bash
# Syntax
aah runcmd <command> <arguments>

# Example of running command `vfs`
aah runcmd vfs find --pattern "conf$"
```
