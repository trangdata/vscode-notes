# Some notes on vscode, aws ec2, ssh, git, python, r

## Local setup

- Install [VSCode](https://code.visualstudio.com/download)

- In `/Users/let20/.ssh/config`:

``` shell
Host TODO
  # Only if needed, uncomment below, set a different username than your local user
  # User <USER>

  # Enable agent forwarding such that local SSH keys are available on the VM.
  # This can be helpful for things like SSH GitHub authentication.
  ForwardAgent yes

  # Will forward port 8888 from the dev VM to local port 8890, which is useful
  # for access to Jupyter Lab
  LocalForward 8890 127.0.0.1:8888
```

## Server setup
Whether you ssh in from VSCode or Terminal, on the AWS server, (_e.g._, RCE v2):

Install [Miniforge 3](https://github.com/conda-forge/miniforge):

```
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```

By doing so, you're setting the default channel to `conda-forge`,
so you can install [programs](https://anaconda.org/conda-forge/python) simply with

```
conda install python
conda install r-base
conda install r-languageserver
conda install r-dplyr
conda install r-seurat
conda install -c bioconda bioconductor-ensdb.hsapiens.v86
```

_Notes_: some packages may not be available on conda-forge, in which case you will need to find a different way to install them (_e.g._, `pip`, `install.packages`)

For a more reproducible workflow, create a new environment with all the necessary packages (with versions) specified in [`environment.yml`](environment.yml):
```
conda env create -f environment.yml
conda activate myenv
```

## Helpful VSCode extensions
- [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter)
- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- [vscode-R](https://marketplace.visualstudio.com/items?itemName=REditorSupport.r)
- [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)


## VSCode for R
Primary extensions:
- [vscode-R](https://marketplace.visualstudio.com/items?itemName=REditorSupport.r): R Extension for Visual Studio Code
- [languageserver](https://github.com/REditorSupport/languageserver): An implementation of the Language Server Protocol for R
- [radian](https://github.com/randy3k/radian): A 21 century R console (Terminal -> Configure terminal settings -> rterm -> `/home/let20/miniforge3/bin/radian`)
    - smarter about running what lines (within a function, for loops, etc.)
    - syntax highlighting/coloring 
    - when arrow "up" to previous function, up by the entire execution, not just one line at a time
    - autocomplete files

## VSCode basics (demo)
- Command Palette: `Cmd Shift P`.
From here, you can go to Remote SSH, Settings, R Addins, Python: Select Interpreter, Convert to .py, Preview markdown, etc.
- Edit shortcuts
- Git
- add Terminals

## RStudio vs VSCode

### RStudio
Things just work out of the box. Minimal customization required.
- Shortcuts: for pipes, finding in scope, create new function
- Ctrl+.: not just files but also functions
- View object by Cmd+Click
- View() especially large dataframe is easy, filter...
- R Workspace/Environment has a lot more information about objects
- help page now has "Run examples" button
- fuzzy search completion
- snippets?

### VSCode
- better git integration
- better multi language support
- function hover

## aws commands

```
# set up keys
aws configure set aws_access_key_id your_access_key --profile riku
aws configure set aws_secret_access_key you_secret_key --profile riku

# example: copy a local file to the researchanalytics bucket
aws s3 --profile riku cp test-trang.txt s3://celgene-rnd-riku-researchanalytics/Evotec/Supplementary/
```

TODO

aws s3 R package

## Additional readings

- https://code.visualstudio.com/docs/editor/workspaces
- https://code.visualstudio.com/docs/languages/r
- RenKun's [video](https://youtu.be/9xXBDU2z_8Y) and accompanying [blogpost](https://renkun.me/2019/12/11/writing-r-in-vscode-a-fresh-start/) on his complete setup
- https://schiff.co.nz/blog/r-and-vscode/
