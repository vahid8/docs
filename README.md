# 👩‍💻 docs

## 📖 Contents
- [Gmail settings for app](#Gmail-app-setting)
- [python virtual env setup](#Python-virtual-env)
- [Kill process using port](#Kill-process-using-port)
- [Supervisor setup](#Super-visor-setup)

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
## Kill Process using Port
 1. find process ID using a specifig port e.g 5000
      ```shell
        $ sudo ss -lptn 'sport = :5000'
      ```
  2. Kill the process
      ```shell
        $ sudo kill -9 <pid>
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
   ### supervisor ClI quick help
    *


    
