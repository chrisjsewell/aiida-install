# AiiDA Install Tutorial

This tutorial is based on the information at: <https://aiida.readthedocs.io/projects/aiida-core/en/latest/intro/get_started.html>

## Creating a Python virtual environment

This is important 

Also Conda: allows for installing non-python packages like postgres, rabbotmq and even [quantumespresso](https://anaconda.org/conda-forge/qe)!

```
python -m venv .venvs/aiida
source .venvs/aiida/bin/activate
pip list
pip install -U pip setuptools wheel
pip install aiida-core
pip list
```

```
verdi
```

will break

```
reentry scan
verdi
verdi status
```

note path of aiida

```
export AIIDA_PATH="/workspaces/aiida-install/.aiida"
verdi status
```

```
echo "export AIIDA_PATH=\"/workspaces/aiida-install/.aiida\"" >> .venvs/aiida/bin/activate
```

tab completion

```
# for mac users
# autoload -Uz compinit && compinit
eval "$(_VERDI_COMPLETE=source verdi)"
```

automate:

```
echo "export AIIDA_PATH=\"/workspaces/aiida-install/.aiida\"" >> .venvs/aiida/bin/activate
echo "$(_VERDI_COMPLETE=source verdi)" >> .venvs/aiida/bin/activate
```

![aiida microservices](./aiida-microservices.png)

```
sudo apt update
sudo apt install postgresql postgresql-server-dev-all postgresql-client rabbitmq-server
sudo chown -R vscode:vscode /var/run/postgresql
/usr/lib/postgresql/11/bin/initdb .aiida/database
/usr/lib/postgresql/11/bin/pg_ctl -D .aiida/database -l .aiida/postgres.log start
```

```
verdi quicksetup --help
verdi quicksetup
verdi status
verdi profile show
verdi database summary -v
```

```
sudo rabbitmq-server -detached start
```

```
verdi status
```


```
verdi daemon start 2
verdi status
verdi daemon status
```

```
verdi archive import "https://archive.materialscloud.org/record/file?filename=HiCond_bands_calculations.aiida&file_id=ee287780-ac04-4b8b-b03c-3cea6e446f9d&record_id=477"
verdi database summary -v
```

```
pip install jupyterlab
cp aiida_magic_register.py ~/.ipython/profile_default/startup/
jupyter lab
```
