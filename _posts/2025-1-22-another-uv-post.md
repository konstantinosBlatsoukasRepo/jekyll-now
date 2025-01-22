---
layout: post
title: another uv post
---

### installing uv

For installing uv, only curl tool required.

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

[uv site](https://docs.astral.sh/uv/#highlights)

### starting a project

```sh
uv init example

## if you want to specify a python version, you can that, using --python option
uv init example --python 3.11
```

this produces a project under a folder name example

```sh
example/
├── .gitignore
├── .python-version
├── README.md
├── hello.py
└── pyproject.toml
```

In the toml file the the required python version is specified (also in the .python-version file).
Also, in the same file the project dependencies are specified, as well.

### running a project

```sh
uv run hello.py
# program output
Using CPython 3.11.11
Creating virtual environment at: .venv
Hello from example!
```

Python 3.11.11 wasn't installed on my local system.
uv downloaded the specified version and also created a virtual environment for us.

Let's examine the folder structure, after the ```uv run``` command.

```sh
.
├── .python-version # default python version
├── .venv
├── README.md
├── hello.py
├── pyproject.toml
└── uv.lock
```

A .venv was created for us, automatically! and uv.lock file (which includes the dependency details, more bellow)

### managing dependencies

You can add/remove projects dependencies in pyproject.toml file, on dependencies list:

```toml
[project]
name = "backend"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "aio-pika==9.5.0",
    "aiormq==6.8.1",
]
```

In the above example, we added two dependencies ```aio-pika``` and ```aiormq```.

After the toml editing, a sync is required (sync updates also the lock file, venv etc.)

```sh
uv sync
```

The other way is to use the following commands:

```sh
uv add 'aio-pika==9.5.0'
uv add 'aiormq==6.8.1'
```

(Imagine that the dependencies were empty) The above commands will add both libs in the dependency list (toml file), update the venv, and the lock file.
Moreover, uv will download and install all the required transitive dependencies.

More info about maanging dependencies, you can find here:

[uv managing dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/)

### deps using pip vs uv

This is a simple test for comparing how much time pip install takes to install libs vs the uv.

Imagine that our current project needs the following libs, to be installed:

```sh
aio_pika
asyncpg
boto3
datadog
fastapi
geojson
httpx
loguru
pydantic_settings
pypika
python-jose
python-multipart
uvicorn
```

Let's try doing this using pip and using uv.

#### using pip

```sh
python -m venv vevn
source vevn/bin/activate
pip install pip-tools
pip-compile requirements.in
pip install --no-cache-dir -r requirements.txt
```

using time in front of the ```pip install``` I got the following time durations:

```sh
real    0m16.852s
user    0m9.211s
sys     0m0.811s
```

#### using uv

All the above libs are included on toml file dependencies list:

```sh
uv sync --no-cache
```

using time in front of the ```uv syn --no-cache```, I got the following time durations:

```sh
real    0m2.553s
user    0m1.511s
sys     0m1.399s
```

comparing the real time, we get a X6.5 reduction time!

### changing python version for a project

```sh
uv python pin 3.13 # no need for python 3.13 to be present (if not, it will be installed automatically)
Updated `.python-version` from `3.11` -> `3.13`
```

let's try now to run again our project

```sh
Using CPython 3.13.1
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Hello from example!
```

CPython 3.13.1, downloaded and installed.
Moreover, previous vevnv automatically deleted and a new one was created.

### not sure if I want to migrate to uv

If you are not sure of using uv, you can get some of the benefits, by just putting ```uv``` word in front of any pip command that you have:

```sh
# use this:
uv pip compile requirements.in -o requirements.txt
# instead of this
# pip-compile requirements.in -o requirements.txt
```

You will get the dependencies much faster.

### python management

#### installing the latest version

```sh
uv install python
```

#### installing specific version

```sh
uv install python 3.12
```

#### installing multiple python versions

```sh
uv install python 3.11 3.12 
```

#### viewing python versions

```sh
uv python list

#output
cpython-3.14.0a4+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.14.0a4-linux-x86_64-gnu                 <download available>
cpython-3.13.1+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.1-linux-x86_64-gnu                   <download available>
cpython-3.12.8-linux-x86_64-gnu                   /home/kostas/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12
cpython-3.11.11-linux-x86_64-gnu                  /home/kostas/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
cpython-3.10.16-linux-x86_64-gnu                  <download available>
cpython-3.10.12-linux-x86_64-gnu                  /usr/bin/python3.10
cpython-3.10.12-linux-x86_64-gnu                  /usr/bin/python3 -> python3.10
cpython-3.10.12-linux-x86_64-gnu                  /usr/bin/python -> /usr/bin/python3
cpython-3.10.12-linux-x86_64-gnu                  /bin/python3.10
cpython-3.10.12-linux-x86_64-gnu                  /bin/python3 -> python3.10
cpython-3.10.12-linux-x86_64-gnu                  /bin/python -> /usr/bin/python3
cpython-3.9.21-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
cpython-3.7.9-linux-x86_64-gnu                    <download available>
pypy-3.10.14-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
pypy-3.7.13-linux-x86_64-gnu                      <download available>
```

You can see the already installed python versions, along the path that each version is installed.
If the python version you specified in the command is already present, this version will be used for executing your python program.