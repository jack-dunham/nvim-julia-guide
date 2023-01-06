# Guide to using Neovim as a Julia IDE

## Installing Julia

First, download and install the appropriate version of Julia accroding to your OS and hardware by following the link: https://julialang.org/downloads/. On macOS I recommend downloading the `.dmg` file, and then following the installation instructions as you would any other application. Then one can either launch the application to begin using Julia (not recommended), or create an alias to the `julia` binary in the application bundle such that Julia can be launched from the command line. Assuming zsh is the shell of choice (default on modern macOS versions), run:
```sh
echo "julia="/Applications/Julia-1.8.app/Contents/Resources/julia/bin/julia"" >> ~/.zshrc
```
This command simply adds the line `julia="/Applications/Julia-1.8.app/Contents/Resources/julia/bin/julia"` to your `.zshrc` file, thus creating the alias. Alternatively, one can add this line manually to the `.zshrc` file in a location of your choice. If using a different shell, then this line must be added to the appropriate config file for that shell. For exmaple, if using plain old bash, one would add the line to the "~/.bashrc" file. If you don't know what shell you are using, then I suggest you google how to find this out.

Now run julia by running the `julia` command. You should see something like this:
```
$ julia
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.8.4 (2022-12-23)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> 
```
First, lets install some packages into your global julia enviroment. In the julia REPL, type `]` to open the package manager interface. You should see something like:
```julia
(@v1.8) pkg>
```
The `(@v1.8)` denotes the current enviroment, in this case the *global* enviroment associated to Julia v1.8. Any project can access the packages installed in this enviroment, regardless of the packages installed in the projects own enviroment. This is useful but can cause problems with conflicting packages, therefore it is *extremely important* not to pollute the global enviroment with random packages for specific tasks you may want to do at random points. Save the global enviroment for a short list of essential and broadly applicable packages. For now, the only package we are going to add is the [Revise.jl](https://github.com/timholy/Revise.jl) package:
```julia
(@v1.8) pkg> add Revise
```
If you accidentally install a package into your global env (it happens) then it can be removed by using `pkg> remove PackageName`. Keep your global environment clean! Now exit julia. 

## Creating an environment for the Julia LSP 

Navigate to your user home directory using Finder, or running `cd ~` from the command line. There should be a hidden directory `.julia` here. To list files and directories from the command line, type `ls -a` where the flag `-a` ensures hidden files and directories are listed. If using Finder, then google how to display hidden files. Within the `.julia` navigate to the `environments` sub-directory and create a directory called `nvim-lspconfig` using the `mkdir` command or within Finder. Navigate into this directory.
