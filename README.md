# Chinese-PDF-to-Text
Convert Chinese PDF to text with Python on Anaconda and Windows

I had a project where I needed to convert a Chinese PDF into text that could be used for analysis.  I had trouble finding packages and resources that were specific to my situation.  I decided to write the prcoess down should I ever need to do this again.  Hopefully this can help you! Before we begin, let's go over my setup:
* Windows 10
* 64-bit OS, x64-based processor
* Anaconda 4.9.2
* Python 3.8.3 (I needed to make another environment with Python 3.7-will go over later)

First, we need to make an environment that is suitable for the trained Chinese language database, [Tesserocr](https://github.com/sirfz/tesserocr). I had to make a seperate virtual environment using Python 3.7.9

Other useful resouces

* https://medium.com/better-programming/beginners-guide-to-tesseract-ocr-using-python-10ecbb426c3d

* https://www.geeksforgeeks.org/python-reading-contents-of-pdf-using-ocr-optical-character-recognition/
