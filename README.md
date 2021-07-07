# AiiDA Install Tutorial

Repository: https://github.com/chrisjsewell/aiida-install

This tutorial is based on the information at: <https://aiida.readthedocs.io/projects/aiida-core/en/latest/intro/get_started.html>

This tutorial is fully reporoducible using a [VS Code Deveopment Container](https://code.visualstudio.com/docs/remote/containers).
It is run using Linux, but should also be applicable to other OS.

## Setting up a Python virtual environment

This is important, the ensure the environment remains correct, it is recommended for any Python based projects.

Using Python `venv` is the simplest way to achieve this, as we do here.
You could aso use [Conda](https://docs.conda.io), which additionaly allows for installing non-python packages like postgres, rabbotmq and even [quantumespresso](https://anaconda.org/conda-forge/qe)!

```
python -m venv .venvs/aiida
source .venvs/aiida/bin/activate
pip list
pip install -U pip setuptools wheel
pip install "aiida-core~=1.6.4"
pip list
pip check
```

When first calling `verdi` you may note that it will error.

```
verdi
```

This is because we first need to call `reentry`, to update all the plugins.

```
reentry scan
verdi
verdi status
```

From the status, you may note that the work path defaults to `.aiida` in your home directory.
When working on multiple AiiDA projects, it may be helpful to change this, using the `AIIDA_PATH` environmental variable.

```
export AIIDA_PATH="/workspaces/aiida-install/.aiida"
verdi status
```

To add tab completion for the `verdi` CLI, run:

```
# for mac users
# autoload -Uz compinit && compinit
eval "$(_VERDI_COMPLETE=source verdi)"
verdi <TAB>
```

To automate the path and completion setup, we can add these two commands to our environment activation script:

```
echo "export AIIDA_PATH=\"/workspaces/aiida-install/.aiida\"" >> .venvs/aiida/bin/activate
echo "$(_VERDI_COMPLETE=source verdi)" >> .venvs/aiida/bin/activate
```

(Note in in VS Code, you can also set "terminal.integrated.env.linux" )

## Setting up the AiiDA services and profile

AiiDA uses a number of external services to run:

![aiida microservices](./aiida-microservices.png)

### Postgres database

```
sudo apt update
sudo apt install postgresql postgresql-server-dev-all postgresql-client rabbitmq-server
# container only command
sudo chown -R vscode:vscode /var/run/postgresql
/usr/lib/postgresql/11/bin/initdb .aiida/database
/usr/lib/postgresql/11/bin/pg_ctl -D .aiida/database -l .aiida/postgres.log start
```

(set `PGDATA` environmental variable, to use pg_ctl without `-D`)

```
verdi quicksetup --help
verdi quicksetup
verdi status
verdi profile show
verdi config list
verdi database summary -v
```

Alternatively, you can use a configuration file:

```
verdi quicksetup --config quicksetup.yml
verdi profile list
```

### Rabbitmq process messaging

This is only required when you want to run processes (not to access data).

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

You can import archives from local files or remote URLs:

```
verdi archive import "https://archive.materialscloud.org/record/file?filename=HiCond_bands_calculations.aiida&file_id=ee287780-ac04-4b8b-b03c-3cea6e446f9d&record_id=477"
verdi database summary -v
```

You can create an archive from one or more computers/codes/groups/nodes:

```
verdi group list -a -A
verdi archive create -G 2 -- myarchive.aiida
verdi archive inspect myarchive.aiida
```

## Using Jupyter Lab

```
pip install jupyterlab
mkdir -p ~/.ipython/profile_default/startup/
cp aiida_magic_register.py ~/.ipython/profile_default/startup/
jupyter lab
```

Now open `Example.ipynb`
