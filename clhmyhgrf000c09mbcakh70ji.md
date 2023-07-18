---
title: "Notebook Setup for Data Science"
datePublished: Sun May 14 2023 05:08:43 GMT+0000 (Coordinated Universal Time)
cuid: clhmyhgrf000c09mbcakh70ji
slug: notebook-setup-for-data-science
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689660086055/b3dc87f8-2a3c-41b2-940e-28a7a4683e8c.png
tags: jupyter-notebook

---

The following are the steps to set up a remote jupyter notebook server that one can connect locally to run their machine learning.

The steps are as follows

### CREATE EXEC SCRIPT

* CREATE a file *jupyter-startup.sh*
    

```bash
source ~/miniconda3/etc/profile.d/conda.sh
conda activate YOUR_CONDA_VIRTUAL_ENVIRONMENT
jupyter notebook
```

### CREATE JUPYTER SERVICE

* CREATE a following jupyter service in your home directory
    
* COPY the file to /*etc/systemd/system/jupyter.service*
    

```bash

[Unit]

Description=Jupyter Notebook Service Description

[Service]

Type=simple

PIDFile=/run/jupyter.pid

ExecStart=/home/YOUR_USER_NAME/jupyter-startup.sh 

User=YOUR_USER_NAME

Group=YOUR_USER_NAME

Restart=always

RestartSec=10

[Install]

WantedBy=multi-user.target
```

```bash
- sudo systemctl enable jupyter.service
- sudo systemctl daemon-reload
- sudo systemctl restart jupyter.service
```

Now you have 2 options

### SSH Tunnel (option 1)

1. Setup a password to the notebook server so that we don't have to provide token every single time. On the remote server, do the following
    

```bash

jupyter notebook password
#Enter password:  ****
#Verify password: ****
#[NotebookPasswordApp] Wrote hashed password to /Users/<<you>>/.jupyter/jupyter_notebook_config.json
```

1. Run SSH tunnel to setup a link on the local host
    

Pick any port number for LOCAL\_PORT. You may need to enter a passphrase.

```bash

ssh -N -f -L localhost:<LOCAL_PORT>:localhost:8080 username@remotemachine -p <<PORT ID>> -v
```

1. Type http://localhost:8888/tree in your local browser to access the jupyter notebook setup remotely.
    

---

### PUBLIC URL (option 2)

#### CONFIG JUPYTER NOTEBOOK

* Create Jupyter Notebook Configuration
    

```bash

jupyter notebook –generate-config
```

##### CREATE PASSWORD and CERTIFICATES

* First generate a new password to login
    

```python
from notebook.auth import passwd
import IPython

passwd()
```

* **Generate the certificate file. It will be used in the configuration step**
    

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
```

![](https://abhisheksreesaila.github.io/blog/images/general/cert-gen.png align="left")

##### EDIT NOTEBOOK CONFIGURATION FILES

```bash
cd .jupyter
nano jupyter_notebook_config.py
```

* **Open the configuration file and edit as follows**
    
    * c.NotebookApp.allow\_origin = '\*'
        
    * c.NotebookApp.allow\_remote\_access = True
        
    * c.NotebookApp.certfile = 'YOUR\_CERTIFICATE\_FILE'
        
    * c.NotebookApp.ip = '0.0.0.0'
        
    * c.NotebookApp.open\_browser = False
        
    * c.NotebookApp.password = u''
        
    * c.NotebookApp.port = ANY\_PORT\_YOU\_LIKE
        

#### EDIT FIREWALL RULES (if needed)

```bash
    sudo ufw allow from <<IP ADDRESS>>
```

# References

* [Config Jupyter Notebook](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html)
    
* [Config Jupyter Notebook 2](https://hyunyoung2.github.io/2017/11/14/How_to_Access_Jupyter_Notebook_Remotely)
    
* [Create Jupyter Service](https://gist.github.com/whophil/5a2eab328d2f8c16bb31c9ceaf23164f)
    
* [Create Jupyter Service 2](https://janakiev.com/blog/jupyter-systemd)
    
* [Configure Linux firewall](https://www.linode.com/docs/guides/configure-firewall-with-ufw)