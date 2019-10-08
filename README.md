# personal-research-env
my personal research environment docker which contains tensorflow, keras, pytorch, and jupyter notebook & lab

## Pre-requisite
- `docker`
- `docker-compose`
- `nvidia driver`
- `cuda 10.0`

## Quickstart
### Clone `ch-shin/personal-research-env` repository
```
cd ~/
git clone https://github.com/ch-shin/personal-research-env.git
cd personal-research-env
```

### Set password to login your Jupyter Notebook
```
export JUPYTER_PASSWD=TYPE_PASSWD_YOU_WANT
```

### Launch Jupyter Notebook with `docker-compose`
This step will takes some minutes for the first time.
```
docker-compose up -d
```

#### Connect to Jupyter Notebook
On browser, connect to `HOST_IT:8888` then type password as you set via environment varible `JUPYTER_PASSWD`.

### If you want to install my dotfiles in jupyter terminal
```
git clone --recursive https://github.com/ch-shin/dotfiles.git ~/.dotfiles
cd ~/.dotfiles && chmod +x setup.sh && bash setup.sh
```

### Turn off Jupyter Notebook and Docker
```
docker-compose down
```
