# Python virtual environments (venv)

Good article: https://frankcorso.dev/setting-up-python-environment-venv-requirements.html

- install with `sudo apt install python3-venv`
- create a _new_ virtual environment (directory) for each project, e.g.
```
cd project
python -m venv .venv
```
- this will create `.venv`, a directory
- activate the venv with `source .venv/bin/activate`
- once it's activated, you can
```
pip install -r requirements.txt
```
- deactivate with `deactivate`
- you can keep an up-to-date requirements.txt file with `pip freeze > requirements.txt`

## notes
- I suppose `.venv` should be included in `.gitignore`, and README instructions should include how to set up and use a `venv` for the project, instead of installing python packages globally