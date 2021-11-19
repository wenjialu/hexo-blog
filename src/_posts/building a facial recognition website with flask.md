---
title: Building a Facial Recognition website with Flask
date: 2021-11-08 02:30:48
tags: AI; Flask
---

I purchase a course from Udemy teaching me from building the machine learning model to integrate it to a website using flask.

I am happy to share my process of learning with you. The thing I need to notice you is that I am not writing every details about the course right now, this note just includes some tips that needs to be noticed and not mentioned by the teacher. If you brought the course, you might find my notes be helpful.



Let's get started ! 

First, open a terminal from jupyter.  (If you are not familar with jupyter and need a tutotial about it, tell me in the comment. I would wirte an acticle to explain it in more detail.)

​	> terminal can be access from here

![image.png](https://tva1.sinaimg.cn/large/008i3skNgy1gwk6ez6nj3j305x06omx3.jpg)

*  Install and import c2
  * if you can not import cv2 in terminal, try use the command "ipython" rather than "python".

* virtual environment
  * create a virtual environment
    * virtualenv
      * since I use ["virtualenvwrapper"](siyuan://blocks/20211112202240-mcpaqeo), instead of using the command in the vedio, Mac user should use this:
        * mkvirtualenv  (environment name: such as freeai)
  * activate it
    * without virtualenvwrapper
      * cd workplace
      * source ./freeai/bin/activate
    * with virtualenvwrapper
      * workon (virtual envir name)
* once activate the virtualenv, you can either run code in the terminal or in jupyter notebook.
  * if you need to use jupyter notebook, simply type "jupyter notebook" in the terminal. Once press enter, it would automatically open a homepage for you.
    

​	

Now, let's move on to the part of building a website that everyone can access!

* Build Flask App in VsCode
  * Open VsCode
    * First things first, enter the virtual environment!!
      * we have created the virtual environment named as freeai before.
        * upgrade
          ```
          deactivate
          pip install --upgrade pip virtualenv virtualenvwrapper
          mkvirtualenv <freeai2>
          ```
      * we would get a package called freeai
      * put the package under the directory of your project.
        
        * ![image.png](https://tva1.sinaimg.cn/large/008i3skNgy1gwk6fvpovxj306f07faa1.jpg)
      * 1.**Ctrl + Shift + P** --> python: select Interpreter
        
        * select the freeai environment
      * 2. File > Preference > Settings,  settings workspace, File:Associations click on "Edit in settings.json"
            1. change "python.pythonPath" to your virtual environment python path (look up by "which python")
                1. `"python.pythonPath":"/Users/lujiawen/workplaces/freeai2/bin/python"`
    * write code
    * run it ![image.png](https://tva1.sinaimg.cn/large/008i3skNgy1gwk6g9f5o2j300x00v0cl.jpg)
      * run it by click the button
      * or in the VScode terminal
        * way1:
          * in vscode ternimal
            * export FLASK_APP=main.py
            * python -m flask run
          * or in ternimal
            * FLASK_APP=main.py falsk run
            * FLASK_APP=main.py FLASK_DEBUG=True falsk run
        * way2:
        * cd to the directory of main.py
        * python main.py
    *
  
* tips:
  * install some modules 
    * which python； python --version
      
      * Although python2.7 is oout of date, it's not recommanded to delete it. We can always use virtual environment to use latest python and mange different versions of packages required by different projects
    * pip3 install opencv-python
      ```
      pip3 install Pillow
  ```
    
      ```
      pip3 install scikit-learn 
    ```
    
  * cheatsheets of postgres
    
    * \q (if you are in the psql);    createdb databasename
    * psql (database user)
    * `/Applications/Postgres.app/Contents/Versions/14/bin/psql -p5432 "postgres"`
    * ![image.png](https://tva1.sinaimg.cn/large/008i3skNgy1gwk6gpvou5j30f803kmxb.jpg)



ref- udemy