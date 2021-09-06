# STDERR Rust Demo 2021-09-06

## Setup

install [Rustup](https://rustup.rs/)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

install [evcxr jupyter kernel](https://github.com/google/evcxr/blob/main/evcxr_jupyter/README.md) 

```bash
rustup component add rust-src
sudo apt install jupyter-notebook cmake build-essential
cargo install evcxr_jupyter
evcxr_jupyter --install
```

launch jupyter

```bash
jupyter-notebook
```

generate config

```bash
jupyter notebook --generate-config
```

set password

```bash
jupyter notebook password --generate-config
```

edit config 


```bash
sudo apt install jq
sh >> ${HOME}/.jupyter/jupyter_notebook_config.py <<EOF
echo 'c.NotebookApp.ip = "*"'
echo 'c.NotebookApp.password = "'$(jq '.NotebookApp.password' ${HOME}/.jupyter/jupyter_notebook_config.json)'"'
EOF
```

install cfitsio

```bash
wget http://heasarc.gsfc.nasa.gov/FTP/software/fitsio/c/cfitsio-3.49.tar.gz
tar -zxvf cfitsio-3.49.tar.gz
cd cfitsio-3.49/
CFLAGS="-O3" ./configure --prefix=/usr/local --enable-reentrant --enable-ssse3 --enable-sse2
make -j
sudo make install
```

references:

https://igiagkiozis.github.io/plotly/content/fundamentals/jupyter_support.html
