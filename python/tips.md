# Python tips and tricks

### Generating a requirements.txt file based on a project directory 

#### Windows
- `python3 -m pip install pipreqs`
- `python3 -m pip install pip-tools`
- `cd /desired/project/directory`
- `python3 -m pipreqs.pipreqs --savepath=requirements.in && python3 -m piptools compile requirements.in`

Then, to install requirements from this generated file on another (Windows) machine:
From the project directory, `python3 -m pip install -r requirements.txt`

#### Linux
- within a venv, use `pip freeze > requirements.txt` (or just `pip list` to view them all)
- much easier