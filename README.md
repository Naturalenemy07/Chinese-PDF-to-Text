# Chinese-PDF-to-Text
Convert Chinese PDF to text with Python on Anaconda and Windows

I had a project where I needed to convert a Chinese PDF into text that could be used for analysis.  I use Jupyter Notebook and had trouble finding packages and resources that were specific to my situation.  I decided to write the prcoess down should I ever need to do this again.  If you are new to data science like myself, hopefully this can help you! Before we begin, let's go over my setup:
* Windows 10
* 64-bit OS, x64-based processor
* Anaconda 4.9.2
* Python 3.8 (I needed to make another environment with Python 3.7-will go over later)

First, we need to make an environment that is suitable for the trained Chinese language database, [tesserocr](https://github.com/sirfz/tesserocr). **tesserocr** is supported by Python 2.7, 3.4, 3.5, 3.6 and 3.7 at the time of writing.  If you already have 3.7, you can proceed to installing tesserocr.  

Making Virtual Environment that uses Python 3.7
======
Assuming you have already installed Anaconda and Python 3.7, make sure to add python 3.7 to your path enviromental variables.  Once this is done, you can open up the Anaconda Prompt.  To create a new virtual environment that contains Python 3.7 type code below into Anaconda.
```
conda create --name <name of your virtual environment> python=3.7
```
Press ``` y ``` in the prompt to procees with environment installation.

Once everything is done you will get a message stating:
```
# To activate this environment, use
#
#     $ conda activate <name of your virtual environment>
```
Before activating, let's verify that the Python actually changes when you switch to the new environment.  Checking the verion of Python before and after activating the new environment will tell you if it was successful.
```
python --version
```
When I typed this into my base environment it returned:
```
(base) C:\Users\JohnnyC>python --version
Python 3.8.3
```
To activate your environment intput:
```
conda activate <name of your virtual environment>
```
Next, i activated the new virtual environment (the name of this environment is pytest37) and it returned:
```
(pytest37) C:\Users\JohnnyC>python --version
Python 3.7.9
```
This verified that the setup of my new virutal environment was successful. Next, [Jupyter Notebook](https://jupyter.org/install) needs to be installed in the new virtual environment.  Make sure that you have activated your new environment.  You should see the name of your environment in parenthesis on the left of the command line.  In the Anaconda Prompt.
```
conda install -c conda-forge notebook
```
Once installed, the python version can be tested in Jupyter Notebook by first opening Jupyter Notebook.  In the new environment, in Anaconda Prompt. 
```
jupyter notebook
```
This will open up a notebook in your web browser.  next we will check the version of Python that the notebook uses.  In Jupyter Notebook, open up a new Python notebook and type in the cell:
```
import sys
print(sys.version)
```
It should return your version of Python 3.7.  For example, my input and output when testing the Python verion was:
```
1 | import sys
2 | print(sys.version)

3.7.9 (default, Aug 31 2020, 17:10:11) [MSC v.1916 64 bit (AMD64)]
```
If your Jupyter Notebook returns a version of Python 3.7, you are good to proceed with installing **tesserocr**!

Installing tesserocr
====================
Now that the hard part is done, proceed to [tesserocr](https://github.com/sirfz/tesserocr) and install the files based on Python version and OS.  

Testing tesserocr
=================

Installing pdf2image
====================

Tesing pdf2image
================

Other useful resouces

1. https://stackoverflow.com/questions/59544006/multiple-versions-of-anaconda-python-installation

2. https://medium.com/better-programming/beginners-guide-to-tesseract-ocr-using-python-10ecbb426c3d

3. https://www.geeksforgeeks.org/python-reading-contents-of-pdf-using-ocr-optical-character-recognition/
