This is a monolithic, full featured [jupyter-notebook](https://pypi.org/project/jupyter-server/#history) server.
All needed packages ready to use for any problem 😁️

```bash
docker pull ghcr.io/tyler71/pythondata:latest
```

What is included:

- Dark themes
- Scientific packages

Kernels
- Bash
- Javascript
- Golang

For a new install, this command needs to be run in the container to fix a theme issue

```bash
docker-compose exec pythondata jt -t onedork -vim
```

## Reverse proxy

Routes to port `8888`

