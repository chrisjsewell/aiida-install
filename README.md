# AiiDA Install Tutorial

Repository: https://github.com/chrisjsewell/aiida-install

This tutorial is based on the information at: <https://aiida.readthedocs.io/projects/aiida-core/en/latest/intro/get_started.html>

This tutorial is fully reporoducible using a [VS Code Deveopment Container](https://code.visualstudio.com/docs/remote/containers).

## Setting up a Python virtual environment

This is important 

Also Conda: allows for installing non-python packages like postgres, rabbotmq and even [quantumespresso](https://anaconda.org/conda-forge/qe)!

```
python -m venv .venvs/aiida
source .venvs/aiida/bin/activate
pip list
pip install -U pip setuptools wheel
pip install "aiida-core~=1.6.4"
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

(can also set "terminal.integrated.env.linux" in VS Code)

## Setting up the AiiDA Services

![aiida microservices](./aiida-microservices.png)

```
sudo apt update
sudo apt install postgresql postgresql-server-dev-all postgresql-client rabbitmq-server
sudo chown -R vscode:vscode /var/run/postgresql
/usr/lib/postgresql/11/bin/initdb .aiida/database
/usr/lib/postgresql/11/bin/pg_ctl -D .aiida/database -l .aiida/postgres.log start
```

(set `PGDATA`, to use pg_ctl without `-D`)

```
verdi quicksetup --help
verdi quicksetup
verdi status
verdi profile show
verdi database summary -v
```

or can use configuration file:

```
verdi quicksetup --config quicksetup.yml
verdi profile list
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

## Using AiiDA archive

```
verdi archive import "https://archive.materialscloud.org/record/file?filename=HiCond_bands_calculations.aiida&file_id=ee287780-ac04-4b8b-b03c-3cea6e446f9d&record_id=477"
verdi database summary -v
```

```
verdi group list -a -A
verdi archive create -G 2 -- myarchive.aiida
```

## Using Jupyter Lab

```
pip install jupyterlab
cp aiida_magic_register.py ~/.ipython/profile_default/startup/
jupyter lab
```
