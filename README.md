# Deploy django project
### using NGINX and Gunicorn
<hr>

## <ins>Table of Content:<ins>
1. [Requirements](#Requirements:)
2. [Install NGINX](#Install-NGINX:)
3. [Prepare environment](#Prepare-environment:)
4. [Prepare project files](#Prepare-project-files:)
5. [Prepare Gunicorn server](#Prepare-Gunicorn-server:)
<hr>

## Requirements:
1. NGINX
2. Gunicorn
<hr>

## Install NGINX:

```shell
# update system
sudo apt update

# install nginx
sudo apt install nginx

# check if nginx installed successfully 
sudo systemctl restart nginx.service
```
<hr>

## Prepare environment:
1. Install pip:
    ```shell
    sudo apt-get install python3-pip
    ```
2. Install virtualenv:
    ```shell
    sudo pip3 install virtualenv
    ```
3. Create directory for the project:
    ```shell
    mkdir project_name
    cd project_name
    ```
4. Now create a virtual environment
    ```shell
    virtualenv project_venv -p 3
    ```
5. Now Activate it:
    ```shell
    source project_venv/bin/activate
    ```
<hr>

## Prepare project files:
1. Upload project to the folder that you just created **project_name**
2. Upload static folder to **project_name**
    ``` 
    .
    └── project_name/
        ├── project/
        │   ├── requirements.txt  
        │   ├── ...
        │   └── project/
        │       ├── ...
        │       ├── settings.py
        │       ├── urls.py
        │       ├── wsgi.py
        │       └── ...
        ├── project_venv/
        │   └── bin/
        │       ├── activate
        │       └── ...
        └── static/
            └── ...
    ```
3. Install requirements:
   ```shell
    pip3 install -r project/requirements.txt
   ```
<hr>


## Prepare Gunicorn server:
1. Install **Gunicorn**:
    ```shell
    pip3 install gunicorn
    ```
2. Create Configuration file
    ```shell
    mkdir conf
    touch conf/gunicorn.conf
    ```
3. Paste this configuration into conf/gunicorn.conf
    ```
    command = '/path_to_project/project_venv/bin/gunicorn'
    #examble: '/home/azureuser/oca/venv/bin/gunicorn'
   
    pythonpath = '/path_to_project'
    #examble: /home/azureuser/oca'
   
    bind = '127.0.0.1:8000' 
    workers = 3

   ```
4. Now run Gunicorn server:
   ```shell
   gunicorn -c conf/gunicorn.py project.wsgi   
   ```
5. Output should be like:
   ```
    [2022-12-05 10:44:14 +0000] [3053259] [INFO] Starting gunicorn 20.1.0
    [2022-12-05 10:44:14 +0000] [3053259] [INFO] Listening at: http://127.0.0.1:8000 (3053259)
    [2022-12-05 10:44:14 +0000] [3053259] [INFO] Using worker: sync
    [2022-12-05 10:44:14 +0000] [3053261] [INFO] Booting worker with pid: 3053261
    [2022-12-05 10:44:14 +0000] [3053262] [INFO] Booting worker with pid: 3053262
    [2022-12-05 10:44:15 +0000] [3053263] [INFO] Booting worker with pid: 3053263
   ```
6. Now your project structure should be like this:
   ```
    .
    └── project_name/
        ├── conf/
        │   └── gunicorn.py
        ├── project/
        │   ├── requirements.txt  
        │   ├── ...
        │   └── project/
        │       ├── ...
        │       ├── settings.py
        │       ├── urls.py
        │       ├── wsgi.py
        │       └── ...
        ├── project_venv/
        │   └── bin/
        │       ├── activate
        │       └── ...
        └── static/
            └── ...
   ```
<hr>
