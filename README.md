<img src="docs/logo.png" align="left" width="192px" height="192px"/>
<img src="" align="left" width="0" height="192px" hspace="10"/>

> The <a href="http://fishshell.com">Fishshell</a> Framework

[![MIT License](https://img.shields.io/badge/license-MIT-007EC7.svg?style=flat-square)](/LICENSE.md) [![Fish Shell Version](https://img.shields.io/badge/fish-v2.2.0-007EC7.svg?style=flat-square)](http://fishshell.com) [![Travis Build Status](http://img.shields.io/travis/oh-my-fish/oh-my-fish.svg?style=flat-square)](https://travis-ci.org/oh-my-fish/oh-my-fish) [![Travis Build Status](https://img.shields.io/badge/gitter-join_chat-brightgreen.svg?style=flat-square)](https://gitter.im/oh-my-fish/oh-my-fish?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Oh My Fish provides core infrastructure to allow you to install packages which extend or modify the look of your shell. It's  fast, extensible and easy to use.

<br><br>

# Install

```fish
curl -L github.com/oh-my-fish/oh-my-fish/raw/master/bin/install | sh
omf help
```

Or _download_ and run it yourself:

```fish
curl -L github.com/oh-my-fish/oh-my-fish/raw/master/bin/install > install
chmod +x install
./install
```

# :beginner: Getting Started

Oh My Fish includes a small utility `omf` to fetch and install new packages and themes.

## `omf update`

Update framework and installed packages.

## `omf install` _`[package|url ...]`_

Install one _or more_ packages.

- You can install packages directly by URL via `omf install URL`
- When called without arguments, install missing packages from [bundles](#dotfiles).

## `omf list`

List installed packages.

## `omf theme` _`<theme>`_

Apply a theme. To list available themes type `omf theme`.

## `omf remove` _`<name>`_

Remove a theme or package.

> Packages subscribed to `uninstall_<pkg>` events are notified before the package is removed to allow custom cleanup of resources. See [Uninstall](#uninstall).

## `omf new pkg | theme` _`<name>`_

Scaffold out a new package or theme.

> This creates a new directory under `$OMF_CONFIG/{pkg | themes}/` with a template.

## `omf submit` _`pkg/<name>`_ _`[<url>]`_

Add a new package. To add a theme use `omf submit` _`themes/<name>`_ _`<url>`_.

Make sure to [send us a PR][omf-pulls-link] to update the registry.

## `omf query` _`<variable name>`_

Use to inspect all session variables. Useful to  dump _path_ variables like `$fish_function_path`, `$fish_complete_path`, `$PATH`, etc.

## `omf destroy`

Uninstall Oh My Fish. See [uninstall](#uninstall) for more information.

# :triangular_flag_on_post: Advanced
+ [Startup](#startup)
+ [Dotfiles](#dotfiles)
+ [Core Library](#core-library)
+ [Packages](#packages)
  + [Creating](#creating)
  + [Submitting](#submitting)
  + [Initialization](#initialization)
  + [Uninstall](#uninstall)
  + [Ignoring](#ignoring)

## Startup

This script runs each time a new session begins, autoloading packages, themes and your _config_ path in that order.

The _config_ path (`~/.config/omf` by default) is defined by `$OMF_CONFIG` in `~/.config/fish/config.fish`. Modify this to load your own configuration, if you have any, as discussed in the [FAQ](FAQ.md#what-does-oh-my-fish-do-exactly).

## Dotfiles

The `$OMF_CONFIG` directory represents the user state of Oh My Fish, and is the perfect
candidate for being added to your dotfiles and/or checked out to version control. There are two important files:

- __`theme`__ - The current theme
- __`bundle`__ - List of currently installed packages/themes

### About the bundle

Everytime a package/theme is installed or removed the `bundle` file is updated. You can also edit it manually and run `omf install` afterwards to satisfy the changes. Please note that while packages/themes added to the bundle gets automagically installed, a package/theme removed from bundle isn't removed from user installation.

## Core Library

The core library is a minimum set of basic utility functions that extend your shell.

+ [See the documentation](/lib/README.md).


## Packages

### Creating

> A package name may only contain lowercase letters and hyphens to separate words.

To scaffold out a new package:

```fish
$ omf new pkg my_package

my_package/
  README.md
  my_package.fish
  completions/my_package.fish
```

> Use `omf new theme my_theme` for themes.

Please provide [auto completion](http://fishshell.com/docs/current/commands.html#complete) for your utilities if applicable and describe how your package works in the `README.md`.


`my_package.fish` defines a single function:

```fish
function my_package -d "My package"
end
```

> Bear in mind that fish lacks a private scope so consider the following options to avoid polluting the global namespace:

+ Prefix functions: `my_package_my_func`.
+ Using [blocks](http://fishshell.com/docs/current/commands.html#block).


### Submitting

Oh My Fish keeps a registry of packages under `$OMF_PATH/db/`.

To create a new entry run:

```fish
omf submit pkg/my_package .../my_package.git
```

Similarly for themes use:

```fish
omf submit theme/my_theme .../my_theme.git
```

This will add a new entry to your local copy of the registry. Please [send us a PR][omf-pulls-link] to update the global registry.


### Initialization

If you want to be [notified](http://fishshell.com/docs/current/commands.html#emit) when your package loads, declare the following function in your `my_package.fish`:

```fish
function init -a path --on-event init_mypkg
end
```

Use this event to modify the environment, load resources, autoload functions, etc. If your package does not export any functions, you can still use this event to add functionality to your package.

### Uninstall

Oh My Fish emits `uninstall_<pkg>` events before a package is removed via `omf remove <pkg>`. Subscribers can use the event to clean up custom resources, etc.

```fish
function uninstall --on-event uninstall_pkg
end
```

### Ignoring

Remove any packages you wish to turn off using `omf remove <package name>`. Alternatively, you can set a global env variable `$OMF_IGNORE` in your `~/.config/fish/config.fish` with the packages you wish to ignore. For example:

```fish
set -g OMF_IGNORE skip this that ...
```


# License

MIT © [Oh My Fish][contributors] :metal:

[fishshell]: http://fishshell.com

[contributors]: https://github.com/oh-my-fish/oh-my-fish/graphs/contributors



[omf-pulls-link]: https://github.com/oh-my-fish/oh-my-fish/pulls
