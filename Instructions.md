# Intrucciones para: 
## Instalar pipenv con cualquier version de python.

### Install pyenv (manages your Python versions)

``` bash
brew update
brew install pyenv
``` 

Add pyenv to your shell (Zsh on macOS)

``` bash
# ~/.zshrc
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

Reload Shell
``` bash
exec $SHELL -l
```
## Install newest version of python.
``` bash
pyenv install 3.xx.x
```
## Install pipenv the clean way (via pipx)
``` bash
brew install pipx
pipx ensurepath
pipx install pipenv
```
Verify
``` bash
pipenv --version
```
# Create a new project pinned to Python 3.12
``` bash
mkdir myproject && cd myproject
# ensure pyenv shims are active (they should be after step 2)
pyenv versions           # sanity check
which python             # should show a pyenv shim path
python -V                # should show 3.12.x

# create Pipenv environment using Python 3.12
pipenv --python 3.12
```
Install packages
``` bash
pipenv install requests  # example
```
Run things inside the env
``` bash
pipenv run python -V     # should show Python 3.12.x
pipenv shell             # optional: activate the env
```


### Later: change the project’s Python version
Say you want to move this project from 3.12 to 3.13 in the future:
Install the new interpreter:

```bash
pyenv install 3.13.x
```
Recreate the Pipenv virtualenv on that interpreter:
```bash
cd myproject
pipenv --rm                    # remove old venv
pipenv --python 3.13           # make a new one
pipenv install                 # re-resolve from Pipfile.lock (or use `pipenv sync`)
```
Pipenv will update [requires].python_version if needed and build a fresh venv.
If you maintain a Pipfile.lock, prefer pipenv sync to reproduce exact versions.
### Working with multiple projects / versions
Per-project pinning: pipenv --python 3.9 or pipenv --python 3.12 as needed.
Temporary override: pyenv shell 3.9.6 (only for current shell).
Project-local default: from the project directory run pyenv local 3.12.x to drop a .python-version file; Pipenv will pick that up when you do pipenv --python 3.12.
### Common gotchas (and fixes)
Using system Python: avoid sudo pip and /usr/bin/python*. Make sure which python shows a path in ~/.pyenv/shims.
Build deps: if a Python build fails, try:
```bash
brew install openssl readline zlib xz sqlite
export LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix zlib)/lib"
export CPPFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix zlib)/include"
pyenv install 3.12.x
```
VS Code: in the Command Palette → “Python: Select Interpreter”, choose the interpreter inside your Pipenv venv (or let the Python extension detect Pipenv automatically). You can also run the project through pipenv run ... tasks.

# TL;DR commands
```bash
# one-time
xcode-select --install
brew install pyenv pipx
echo 'export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"' >> ~/.zshrc
exec $SHELL -l

pyenv install 3.12.x
pyenv global 3.12.x

pipx ensurepath
pipx install pipenv

# per project
mkdir myproject && cd myproject
pipenv --python 3.12
pipenv install requests
pipenv run python -V
