# Matlab Package Installer (mpi)

[![View mpi on File Exchange](https://www.mathworks.com/matlabcentral/images/matlab-file-exchange.svg)](https://www.mathworks.com/matlabcentral/fileexchange/54548-mpm)

A simple package manager for Matlab (inspired by [pip](https://github.com/pypa/pip)). Downloads packages from Matlab Central's File Exchange, GitHub repositories, or any other url pointing to a .zip file.

## Quickstart

Download/clone this repo and add it to your Matlab path (using `addpath`). Now try the following:

- `mpi install [package-name]`: install package by name
- `mpi uninstall [package-name]`: remove package, if installed
- `mpi search [package-name]`: search for package given name (checks Github and Matlab File Exchange)
- `mpi freeze`: lists all packages currently installed
- `mpi init`: adds all installed packages to path (run when Matlab starts up)

## More details

### Install a single package

__Install (searches FileExchange and Github):__

```
>> mpi install export_fig
```

When installing, mpi checks for a file in the package called `install.m`, which it will run after confirming (or add `--force` to auto-confirm). It also checks for a file called `pathlist.m` which tells it which paths (if any) to add.

__Install a Github release (by tag, branch, or commit)__

By tag:

```
>> mpi install matlab2tikz -t 1.0.0
```

By branch:

```
>> mpi install matlab2tikz -t develop
```

By commit:

```
>> mpi install matlab2tikz -t ca56d9f
```

__Uninstall__

```
>> mpi uninstall matlab2tikz
```

When uninstalling, mpi checks for a file in the package called `uninstall.m`, which it will run after confirming (or add `--force` to auto-confirm).

__Search without installing:__

```
>> mpi search export_fig
```

__Install from a url:__

```
>> mpi install covidx -u https://www.mathworks.com/matlabcentral/fileexchange/76213-covidx
```
OR:

```
>> mpi install export_fig -u https://github.com/altmany/export_fig.git
```

(Note that when specifying Github repo urls you must add the '.git' to the url.)

__Install local package:__

```
>> mpi install my_package -u path/to/package --local
```

The above will copy `path/to/package` into the default install directory. To skip the copy, add `-e` to the above command.

__Overwrite existing packages:__

```
>> mpi install matlab2tikz --force
```

__Install/uninstall packages in a specific directory:__

```
>> mpi install matlab2tikz -d /Users/mobeets/mypath
```

Note that the default installation directory is `mpi-packages/`.

## Environments ("Collections")

mpi has rudimentary support for managing collections of packages. To specify which collection to act on, use `-c [collection_name]`. Default collection is "default".

```
>> mpi install cbrewer -c test
Using collection "test"
Collecting 'cbrewer'...
   Found url: https://www.mathworks.com/matlabcentral/fileexchange/58350-cbrewer2?download=true
   Downloading https://www.mathworks.com/matlabcentral/fileexchange/58350-cbrewer2?download=true...
>> mpi init -c test
Using collection "test"
   Adding to path: /Users/mobeets/code/mpi/mpi-packages/mpi-collections/test/cbrewer
   Added paths for 1 package(s).
```

## Installing multiple packages from file

```
>> mpi install -i /Users/mobeets/example/requirements.txt
```

Specifying a requirements file lets you install or search for multiple packages at once. See 'requirements-example.txt' for an example. Make sure to provide an absolute path to the file!

To automatically confirm installation without being prompted, set `--approve`. Note that this is only available when installing packages from file.

## What it does

By default, mpi installs all Matlab packages to the directory `mpi-packages/`. (You can edit `mpi_config.m` to specify a custom default installation directory.)

If you restart Matlab, you'll want to run `mpi init` to re-add all the folders in the installation directory to your Matlab path. Better yet, just run `mpi init` from your Matlab [startup script](http://www.mathworks.com/help/matlab/ref/startup.html).

## Troubleshooting

Because there's no standard directory structure for a Matlab package, automatically adding paths can get a bit messy. When mpi downloads a package, it adds a single folder within that package to your Matlab path. If there are no `*.m` files in the package's base directory, it looks in folders called 'bin', 'src', 'lib', or 'code' instead. You can specify the name of an internal directory by passing in an `-n` or `internaldir` argument. To install a package without modifying any paths, set `--nopaths`. Or to add _all_ subfolders in a package to the path, set `--allpaths`.

mpi keeps track of the packages it's downloaded in a file called `mpi.mat`, within each installation directory.

## Requirements

mpi should work cross-platform on versions Matlab 2014b and later. Also note that, starting with Matlab 2022, you may see a warning when using mpi, as Matlab includes a built-in command of the same name (used for installing Matlab products). You may need to rename the file `mpi.m` to something else, and then rename the function name on line 1 of this file to match, as well as the line containing "help mpi".
