# 👩‍💻 docs

## 📖 Contents
- [Gmail settings for app](#Gmail-app-setting)
- [python virtual env setup](#Python-virtual-env)

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
