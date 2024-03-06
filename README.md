# cookiecutter-ml-workspace

A [cookiecutter](https://github.com/cookiecutter/cookiecutter) template for machine learning works.

## Usage

### Options

* `author_name`: your name
* `project_name`: your project name
* `project_description`: description of your project (this will be inserted to `README.md`)
* `use_cuda`: use cuda
* `jupyter_port`: a port to access JupyterLab running on a container
* `tensorboard_port`: a port to access TensorBoard running on a container

### Generate project and launch Docker container

Run following commands:

```bash
$ cookiecutter https://github.com/eqs/cookiecutter-ml-workspace
# cd to generated project dir
$ docker-compose build  # (or docker-compose build --no-cache)
$ docker-compose up -d
```

Access following URLs from web browser:

* `localhost:<jupyter_port>`
* `localhost:<tensorboard_port>`
