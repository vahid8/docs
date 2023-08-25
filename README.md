# 👩‍💻 docs

## 📖 Contents
- [Gmail settings for app](#Gmail-app-setting)
- [Supervisor setup](#Supervisor-setup)
- [Ubuntu Add User](#Ubuntu-Add-User)
- [Nginx](#nginx)
- [Secure Nginx Encrypt](#Secure-nginx-encrypt)
- [Shortcuts](#Shortcuts)
- [Git](#Git)
- [Docker](#Docker)
- [Alembic](#alembic)
- [FastAPI](#fastapi)
- [Conda](#conda)
- [MongoDB](#mongodb)
- [GoogleCloud](#googlecloud)

## Gmail App Setting
use the following procedure to get a 16 digit code to
be used the password for the email

  - Do the 2 step verification for the Email
  - Go to app password
  - Select custm and generate a 16 digit password
  - use this 16 digit without space in the python code as gmail passeord


## Supervisor setup
  partly reference in  [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-manage-supervisor-on-ubuntu-and-debian-vps)
 1. Install and restart it
      ```shell
        $ sudo apt-get install supervisor
        $ sudo service supervisor restart
      ```
  2. Add a new program 
      write a bash code like below example.
      Addin shebang (#!/bin/bash) is very important also cd into your working dir
      ```shell
        #!/bin/bash
        cd /home/dev2/Vahid_codes/web_app/flask_blurring
        source /home/dev2/Vahid_codes/env/bin/activate
        export APP_SETTINGS="project.config.ProductionConfig"
        #python manage.py runserver --host=0.0.0.0
        gunicorn -b 0.0.0.0:5000 -w 6 project:app --timeout 36000
      ```
      and make it executable using the nelow command.      
      ```shell
        $ chmod +x <path to your .sh file>
      ```
   3. Make sure the following folder exist
      * /var/log/
   4. Add new config for your program in supervisor config folder
      * /etc/supervisor/conf.d/<arbitrary_name_for_programm_task e.g face_script>.conf
      * copy the following into the created config file
      ```yaml
        [program:face_script]
        command=<path to your bash.sh file>
        autostart=true
        autorestart=false # change it to true for production level, in development I want to stop process manually and dont start it agin automatically
        stopasgroup=true # To stop all subprocess when it stop or restarts 
        stderr_logfile=/var/log/face_script.err.log
        stdout_logfile=/var/log/face_script.out.log
      ```
   5. update the supervisor   
      ```ssh
        $ sudo supervisorctl reread
        $ sudo supervisorctl update
      ```
      Then it will automatically start your program. you can check logs in your log file.
      
   6. in case of changing configs or bash files:
      ```ssh
        $ sudo supervisorctl restart
      ```
   #### supervisor ClI quick help
    * Enter shell : $ sudo supervisorctl  
    * See available options: >>help
    * see status: >>status
    * stop a task: >>stop <task_name>
    * start a stoped one: >>stop <task_name>
    * restart a task: >>restart <task_name>
    * quit shell: >>quit

## Ubuntu Add User 
### Add new user with Administrative Privileges
partly reference in  [Digital Ocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)
1. Logging in as root:
```shell
$ ssh root@your_server_ip
```
2.create a new user
```shell
  $ adduser my_new_user
```
-> check it
```
getent passwd {1000..60000} | cut -d: -f1
```

3.Granting Administrative Privileges (adding to sudo list)
```shell
  $ usermod -aG sudo my_new_user
```
-> check it 
```shell
getent group sudo | cut -d: -f4
```
4. Copy your local machine public key to the server using terminal on your local machine (if you need to access your server with ssh directly instead of password)
```
ssh-copy-id my_new_user@ip.address
```

5. if you allready used the root in server, you need to copy everything from authorized keys of the root to your new user  
```shell
  $ rsync --archive --chown=my_new_user:my_new_user ~/.ssh /home/my_new_user
```
5. you can login to server as non -rrot using :
```ssh
$ ssh my_new_user@your_server_ip
```
## Nginx
Install it on your system
#### change the config
inside /etc/nginx folder
```
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup # keep copy of default config as backup
sudo cp my_config.conf nginx.conf
sudo nginx -t -c /etc/nginx/nginx.conf  # test the config
sudo systemctl reload nginx    # or sudo systemctl start nginx 
```
### routing
use nginx ---> localhost:80/f_api/add?x=3&y=7
or 
use open original port ---> localhost:8000/add?x=2&y=4

f_Api is not written on fast api's scripts but you need to adress it in your app (e.g flask or node.js) and nginx config to search for it on port 8000. 

### others
solve problem with static access in case you create a new user <your_user> rather than root
(check the nginx file here for more info)
```
sudo usermod -a -G your_user www-data
sudo chown -R :www-data /path/to/your/static/folder
```

## Secure Nginx Encrypt
**Prerequisites**
 1- having a domain name
 2- add it to server
 3- having two DNS record like below on your domain settings in the server:
 | Type | Hostname | Value | TTL |
| --- | --- | --- | --- |
| A | www.your_domain_name.com | server_ip_address | 3600 |
| A | your_domain_name.com  | server_ip_address | 3600 |

**1.Install certbot**
```sh
sudo apt install certbot python3-certbot-nginx
```
**check** to see your domain name in /etc/nginx/sites-available/your_domain_name
or in etc/nginx/sites-enabled/your-servername
you need to have serve_name your-servername.com www.your-servername.com;
under the listen parameters in server configurations.
*check* for typos in server config by : 
```sh
$ sudo nginx -t
```
**2.reload nginx**
```sh
$ sudo systemctl reload nginx
```
**3.Allowing HTTPS throught the firewall**
```
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```
**4.finally obtain an SSL certificate using certbot**
```
sudo certbot --nginx -d example.com -d www.example.com
```
**5.optional : activate auto-renewal (certificates are 90 days valid)**
```
sudo systemctl status certbot.timer
```




## Shortcuts
here is the list of commands that will be needed in daily routines
| Command | Description |
| --- | --- |
| `ip r` | get the ip of current machine |
| `curl -4 icanhazip.com` | get my public id  |
| `Ctrl+f5` | remove cache |
| `unzip file.zip -d destination_folder` | unzip a file |
| `sudo rm -r folder_path` | delete a folder recursively |
| `sudo ss -lptn 'sport = :5000'` | find pid of the process that use a specifig port e.g 5000|
| `sudo ps -a` | see list of running processes |
| `ps aux \| grep pycharm` | search for a specific process like pycharm|
| `sudo kill -9 <pid>` | kill a process |
| `watch -n 1 nvidia-smi` | check the status of GPU with 1 second update |
| `sudo du -cha --max-depth=1 / \| grep -E "M\|G"` | Check the memory consumpation of files
| `sudo du -sh` | Check the memory consumtion of a directory
| `lspci -v \| less` | Check the Graphic card spec (use q for exit)
| `ubuntu-drivers devices` | Check the Graphic card spec (use q for exit)
| `nproc --all` | Number of cpu cores
| `cat /etc/os-release` | check the os spec
| `df -h /` | check the os Harddisk free space
| `scp -P 22884 /home/vahid/Downloads/libcudnn8_8.2.deb  ddevroot@192.20.100.158:/home/ddevroot/uploads/libcudnn8.deb` | copy file to server
| `sudo apt-get install python3-tk` | in case of no gui installed for plt.show()
| `sudo fdisk -l` | To get list of all HDD (internal and external with their name
| `sudo mount -o remount,uid=1000,gid=1000,rw /dev/sdd1`| To remount a spesific HDD with write privilege
| `sudo apt-get install lxde` | To install lxde ( A lightweight desktop environment for ubuntu

 



## Git
How to add ssh key :
```ssh
1. Find your public ssh key (if exists) -> cd .ssh and ls
if not exists -> ssh-keygen to create
2. Cat <the-name>.pub
3. copy that starting with ssh-rsa ...
4. Go to github settings and add new ssh then paste it there.
```
here is the list of commands that will be needed in daily routines
| Command | Description |
| --- | --- |
| `git dif <filename>` | see the changes to a file from previus commit till now |
| `git checkout -- <filename>` | discard local changes on a specific file |
| `git checkout -- . or git reset --hard` | discard all local changes on a repo |
| `git checkout --track origin/master` | In case you get in to the detached head instead of the remote branch |
| | It tells stricktly that you need to track the branch master from remote |  


## Docker
* In docker version 2 "docker-compose" is changed to "docker compose"
* install docker and docker compose -> https://docs.docker.com/desktop/install/ubuntu/
* install gpu enebaled docker -> https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

| Command | Description |
| --- | --- |
| ```docker exec -t your-db-container pg_dumpall -c -U postgres > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql``` | backup the database running on docker |
| ```cat your_dump.sql \| docker exec -i your-db-container psql -U postgres``` | Restore the database to the docker |
| ```docker build -t test_pcl .``` | Build an image with the name test_pcl using docker file in the current dir |
| ```docker run -it --rm test_pcl /bin/bash``` | Brun the image and remove the docker after exit |
| ```docker images``` | Available images on the system |
| ```docker ps -a``` | Available containers on the system (running or stopped) |
| ```docker ps``` | Available containers on the system (running) |
| ```docker rm``` | remove one or more container |
| ```docker rmi``` | remove one or more images |
| ```docker compsoe up -d``` | creating and running docker in detach mode |
| ```docker-compose run --rm detectron2``` | create the docker based on the image and attatch to it and remove container after exit |
| ```docker-compose run --rm yolo``` | create the docker based on the image and attatch to it and remove container after exit |
| ```docker run -it -v localpath:containerpath #Image:tag``` |  create a docker from an image and connect to it with volume definition |
| ```docker exec -it <name of docker> /bin/bash``` | Now you can run your command including python3 commands.py create-db |

### use docker for database
1- How to use postgres terminal:
```
docker copmose up -d
docker exec -it < docker-name> -U <username(postgres or flaskuser or ...)> 
```
2- Restart database (remove network)
```
docker compose down
docker compose up
```
3- change the name of localhost to youyr specified name (postgis here) in docker-compose yaml for the database.

## alembic
To do: create table
```
docker-compose up -d website 
docker ps
docker exec -it CONTAINER_ID /bin/bash
pip install alembic
export APP_SETTINGS="project.config.ProductionConfig"
python3 commands.py drop-db
alembic upgrade head
```

## Conda
1. Install anaconda mini
2. open anaconda prompt windows
```
conda create -n geo_env
conda activate geo_env
//conda config --env --set channel_priority strict
conda install -c conda-forge geopandas
conda install -c open3d-admin open3d
conda install -c conda-forge laspy
conda install -c conda-forge opencv
conda install -c conda-forge scikit-learn
```
* To change dir in windows:
```
cd /d d:\
```

## MongoDB
```
from pymongo import MongoClient
client = MongoClient("mongodb://db:27017")
db = client.SentencesDatabase  # create or connect to an existing database (here Sentence database)
users = db["Users"]  # create or connect to an existing collection (here sentences collection)
```

## GoogleCloud
#### Installation
download it from  https://cloud.google.com/sdk/docs/install
Initialize it

#### Log in
gcloud auth login

#### Storage (gsutil)
| Command | Description |
| --- | --- |
| ```gsutils ls``` | name of avilable buckets inside that project |
| ```gsutil ls gs://my-awesome-bucket``` | list of folders and files inside a specific bucket |
| ```gsutil mv gs://my-awesome-bucket/images gs://my-awesome-bucket/imagesOriginal``` | move files |
| ```gsutil -m mv gs://my-awesome-bucket/images gs://my-awesome-bucket/imagesOriginal``` | move files multithread |


#### cloud (cloud cli)
| Command | Description |
| --- | --- |
| ```gcloud config set project PROJECT_ID``` | change project |


#### Popstgres ()

