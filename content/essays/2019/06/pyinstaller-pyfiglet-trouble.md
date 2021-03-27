---
title: "PyInstaller + PyFiglet = Trouble"
date: 2019-06-01T19:14:00+00:00
draft: false
summary: This essay is a slightly extended version of a [comment](https://github.com/pyinstaller/pyinstaller/issues/2389#issuecomment-476414044) I made on `PyInstaller`'s official repository, regarding `PyInstaller` and `PyFiglet` integration, as well as some known issues.
categories:
  - oss
  - python
---

This essay is a slightly extended version of a [comment](https://github.com/pyinstaller/pyinstaller/issues/2389#issuecomment-476414044) I made on `PyInstaller`'s official repository, regarding `PyInstaller` and `PyFiglet` integration, as well as some known issues.

[PyFiglet](https://github.com/pwaller/pyfiglet) allows you to create _really_ large letters out of ordinary text, as per [figlet](http://www.figlet.org/)'s own description, which essentially results in interesting displays commonly seen in command line interfaces. One example of this would be:

```shell
# From wwww.figlet.org.
 _ _ _          _   _     _     
| (_) | _____  | |_| |__ (_)___ 
| | | |/ / _ \ | __| '_ \| / __|
| | |   <  __/ | |_| | | | \__ \
|_|_|_|\_\___|  \__|_| |_|_|___/
```

Whenever you want to freeze a Python package with `PyInstaller`, `PyInstaller` will run a series of built-in or custom hooks which define what a specific package needs in order to be _freezable_. `PyFiglet` doesn't have a built-in hook in `PyInstaller` so someone [suggested](https://github.com/pyinstaller/pyinstaller/issues/2389) that a hook should be created so that everyone could use `PyInstaller` to freeze applications that use `PyFiglet`. This is where things started to become troublesome.

As it appears, `PyFiglet` uses `pkg_resources`as a way to find out which fonts you would like to use for the displayed text. A runtime hook in `PyInstaller` - these are different from the pre-runtime hooks mentioned above - is registering `pkg_resources.NullProvider` which means that everything that uses `pkg_resources` will forcibly use the `NullProvider`, triggering the error: `NotImplementedError: Can't perform this operation for unregistered loader type`. In essence, this means that the registered provider for `pkg_resources` is incompatible with importing _some_ packages for freezing.

Since, we cannot register a new provider, as the `NullProvider` will always be registered first, and we also can't _easily_ change the core of `PyInstaller`, in order to register a different provider, I've come upon a workaround that builds on top of the suggestions in the mentioned discussion.

We start by adding a custom hook to be run by `PyInstaller`, suggested in the [initial issue](https://github.com/pyinstaller/pyinstaller/issues/2389#issue-201124527):

```python
# hook-pyfiglet.py

from PyInstaller.utils.hooks import collect_all
datas, binaries, hiddenimports = collect_all("pyfiglet")
```

This will force `PyInstaller` to import all known `PyFiglet` modules, as well as data. Following that, we need to force registering a new provider, somewhere inside the script that will be frozen, before any use of `pyfiglet` occurs. An example of this will be:

```python
import pkg_resources
import sys

from pyimod03_importers import FrozenImporter

if getattr(sys, 'frozen', False):
   pkg_resources.register_loader_type(
       FrozenImporter, pkg_resources.DefaultProvider
   )
```

As I said in my comment, in essence, instead of registering `NullProvider`, we now register `FrozenImporter` with `DefaultProvider`, which inherits from `NullProvider` anyway - but doesn't error apparently.  I couldn't figure out any side effects from this but **I would advise caution**, when using this workaround, as well as thorough testing, to guarantee that there are no side effects that affect the script that is going to be frozen.

 
