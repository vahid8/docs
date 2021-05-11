# 👩‍💻 docs

## 📖 Contents
- [Gmail settings for app](#Gmail-app-setting)
- [python virtual env setup](#Python-virtual-env)
- [Supervisor setup](#Supervisor-setup)
- [Server setup](#Server-setup)
- [Shortcuts](#Shortcuts)

## Gmail App Setting
use the following procedure to get a 16 digit code to
be used the password for the email

  - Do the 2 step verification for the Email
  - Go to app password
  - Select custm and generate a 16 digit password
  - use this 16 digit without space in the python code as gmail passeord


## Python Virtual Env
use the following procedure to setup a new virtual env
  1. Install venv package using pip
      ```shell
        $ sudo apt install python3-venv
      ```
  2. Make a new direvtory for creating files inside
      ```shell
        $ mkdir my_env
      ```
  3. Create a virtual envirnment in this dir
      ```shell
        $ python3 -m venv my_env
      ```
  4. Acivate the envirnment
      ```shell
        $ source my_env/bin/activate
      ```
  * To install packages listed in requirments.txt
      ```shell
        $ pip install -r requirments
      ``` 
  * To deactivate the virtual environment
      ```shell
        $ deactivate
      ```
  * To get list of packages installed inside a virtual envirnment
      ```shell
        pip freeze
      ```
  * To save them in a txt
      ```shell
        pip freeze > requirments.txt
      ```


## Supervisor Setup 
  partly reference in  [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-manage-supervisor-on-ubuntu-and-debian-vps)
 1. Install and restart it
      ```shell
        $ sudo apt-get install supervisor
        $ sudo service supervisor restart
      ```
  2. Add a new program 
      write a bash code (I will include it to my projects on Github just change the virutual env paths).     
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
        autorestart=false # change it to true for production level
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
   #### supervisor ClI quick help
    * Enter shell : $ sudo supervisorctl  
    * See available options: >>help
    * see status: >>status
    * stop a task: >>stop <task_name>
    * start a stoped one: >>stop <task_name>
    * restart a task: >>restart <task_name>
    * quit shell: >>quit

## Server Setup 
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
3.Granting Administrative Privileges (adding to sudo list)
```shell
  $ usermod -aG sudo my_new_user
```
4.Enabling External Access using ssh
  you need to add a copy of your local public key to the new users ~/.ssh/authorized_keys file
  or simply use rysinc to copy it from root user ( you already have the key there) to the new user
```shell
  $ rsync --archive --chown=my_new_user:my_new_user ~/.ssh /home/my_new_user
```
5. you can login to server as non -rrot using :
```ssh
$ ssh my_new_user@your_server_ip
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
| `sudo kill -9 <pid>` | kill a process |



    
