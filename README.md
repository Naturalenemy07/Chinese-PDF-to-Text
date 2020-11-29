# Chinese-PDF-to-Text
Convert Chinese PDF to text with Python on Anaconda and Windows using Jupyter Notebook.

I had a project where I needed to convert a Chinese PDF into text that could be used for analysis.  I use Jupyter Notebook and had trouble finding packages and resources that were specific to my situation.  I decided to write the prcoess down should I ever need to do this again.  If you are new to data science like myself, hopefully this can help you! Before we begin, let's go over my setup:
* Windows 10
* 64-bit OS, x64-based processor
* Anaconda 4.9.2
* Python 3.8 (I needed to make another environment with Python 3.7-will go over later)

First, we need to make an environment that is suitable for the trained Chinese language database, [tesserocr](https://github.com/sirfz/tesserocr). **tesserocr** is supported by Python 2.7, 3.4, 3.5, 3.6 and 3.7 at the time of writing.  If you already have 3.7, you can proceed to installing tesserocr.  

Making Virtual Environment that uses Python 3.7
======
Assuming you have already installed Anaconda and Python 3.7, make sure to add Python 3.7 to your path enviromental variables.  Once this is done, you can open up the Anaconda Prompt.  To create a new virtual environment that contains Python 3.7 type code below into Anaconda Prompt.
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
Before activating, let's verify that the Python version actually changes when you switch to the new environment.  Checking the verion of Python before and after activating the new environment will determine if it was successful.
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
Next, I activated the new virtual environment (the name of this environment is pytest37), and checked my Python version, it returned:
```
(pytest37) C:\Users\JohnnyC>python --version
Python 3.7.9
```
This verified that the setup of my new virutal environment was successful. Next, [Jupyter Notebook](https://jupyter.org/install) needs to be installed in the new virtual environment.  Make sure that you have activated your new environment before installing Jupyter.  You should see the name of your environment in parenthesis on the left of the command line.  In the Anaconda Prompt:
```
conda install -c conda-forge notebook
```
Once installed, the Python version can be tested in Jupyter Notebook by first opening Jupyter Notebook.  In the new environment, in Anaconda Prompt. 
```
jupyter notebook
```
This will open up a notebook in your web browser.  Next we will check the version of Python that the notebook uses.  In Jupyter Notebook, open up a new Python notebook and type in the cell:
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
Now that the hard part is done, proceed to [tesserocr](https://github.com/sirfz/tesserocr) and install the files based on Python version and OS.  This folder will read **tesserocr-master**.  I installed and unzipped the files in the directory which opens up when I activate my Python 3.7 virtual environment in Anaconda.  To ensure this, use conda to install **tesserocr**.
```
conda install -c conda-forge tesserocr
```
A wheel file appropriate to your Windows version needs to be installed.  Go to [simonflueckiger/tesserocr-windows_build](https://github.com/simonflueckiger/tesserocr-windows_build/releases), click and download the file appropirate for your system in the **tesserocr-master** folder that was installed in the previous step.  For my system, I downloaded the **tesserocr-2.4.0-cp36-cp36m-win_amd64.whl** file.  Next the file needs to be installed via pip by:
```
pip install <package name>.whl
```
For me I typed in:
```
pip install tesserocr-2.4.0-cp36-cp36m-win_amd64.whl
```
Next you need to install **Pillow** which is the Python Imaging Library.  More information can be found at [Pillow](https://pillow.readthedocs.io/en/stable/).  You can pip install **Pillow** in the Anaconda prompt by:
```
pip install Pillow
```
Next, you need to install a trained language packages from tessdata.  There are three levels: standard, best and fast. Click the below links to download and learn more about the trained dataset.  When you download, make sure to put this folder in the **tesserocr-master** folder.  
1. [tessdata_standard](https://github.com/tesseract-ocr/tessdata) -  only works with Tesseract 4.0.0.
2. [tessdata_best](https://github.com/tesseract-ocr/tessdata_best) -  only works with Tesseract 4.0.0.
3. [tessdata_fast](https://github.com/tesseract-ocr/tessdata_fast)

I downloaded the **tessdata_best** package, and renamed it **tessdata**.  The "best" file is fairly large, is a little slower to use by the algorithm, but has a higher accuracy.  We will only use the Simplified Chinese language pack, but **tessdata** can recognize traditional chinese and over 100 other lanugages. More information can be found at [tesseract-ocr](https://github.com/tesseract-ocr/tesseract).  

Testing tesserocr
=================
**Tesserocr** uses .jpg/.jpeg/.png images as input and converts the picture to text.  Before we can convert a PDF file to text, we need to test the ability to convert the image to text. First, download the image at [image](https://github.com/Naturalenemy07/Chinese-PDF-to-Text/blob/main/chinese_test_file.png).  You can also use your own image if preferred.  Make sure to save this image to the same directory that opens up when you open the Jupyter Notebook in the virtual environment that uses Python 3.7-whether you made a new one from the above steps or already had this environment suitable for tesserocr.  This is important for the program to find the image-I'm unsure if you can input a path in the Image.open() function.  

Now, activate this environment, and open Jupyter Notebook. First, this image gets preprocessed into a black and white image.  
```
from PIL import Image
column = Image.open(<name of input file>)
gray = column.convert('L')
blackwhite = gray.point(lambda x: 0 if x < 200 else 255, '1')
blackwhite.save(<name of output file>)
```
My input into Jupyter Notebook was:
```
1 | from PIL import Image
2 | column = Image.open('chinese_test_file.png')
3 | gray = column.convert('L')
4 | blackwhite = gray.point(lambda x: 0 if x < 200 else 255, '1')
5 | blackwhite.save('code_bw.jpg')
```
Now that the image has been preprocessed, it can be analyzed and converted to text.  Note, the language package is the tessdate_standard, tessdata_best, or tessdata_fast file downloaded in previous step.  As recommended, it should be in the tesserocr-master folder.  
```
from tesserocr import PyTessBaseAPI
with PyTessBaseAPI(path=r'path-to-language-package', lang='chi_sim') as api:
    api.SetImageFile(<name of output file>)
    print(api.GetUTF8Text())
```
My input into Jupyter Notebook is below.
```
1 | from tesserocr import PyTessBaseAPI
2 | with PyTessBaseAPI(path=r'C:\Users\JohnnyC\tesserocr-master\tessdata', lang='chi_sim') as api:
3 |     api.SetImageFile('code_bw.jpg')
4 |     print(api.GetUTF8Text())
```
In my language package path in line 2 above, I had to put an "r" before the string.  If I didn't I would get a **SyntaxError** as seen below:
```
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 2-3: truncated \UXXXXXXXX escape
```
In line 4, I printed the text to test the function.  The first few lines from the output should be:
```
科学 技术 保密 规定

(1995 年 1 月 6 日 国家 科学 技术 委员 会 、 国 家 保密 局 发 布
国家 科学 技术 委员 会 、 国 家 保密 局 令 第 20 号 )
...
```
More information about post-processing and customizing can be found in the "Other userful resources" section at the end of this readme, or the **tesserocr** source github page. 

Installing pdf2image and poppler-windows
========================================
Now that the functions can convert from a picture to text, we can now build on this to convert a pdf to text.  We will use the [pdf2image](https://pypi.org/project/pdf2image/) package to accomplish this.  In the same virtual environment,using the Anaconda prompt:
```
pip install pdf2image
```
**pdf2image** requires **poppler**, it can be downloaded at [poppler-windows](https://github.com/oschwartz10612/poppler-windows/releases/).  Make sure to add the poppler file to your environmental path.  I also put the folder in the same directory as **tesserocr-master**.  Note, I downloaded poppler-0.90.1. I haven't tested Chinese-PDF-to-text with more updated versions of **poppler**.    

Testing pdf2image
================
Now that **pdf2image** and **poppler** is installed, we can test the ability to convert Chinese text in a PDF document to text.  Note, this method currently will convert the PDF document to a .jpg/.jpeg image(which is saved on your computer), then convert the chinese text in that image file to a text your computer can understand.  In the future, I will figure out how to skip the creation of a .jpg/.jpeg file and go straight to text.  Next, a Chinese PDF can be downloaded [here](https://github.com/Naturalenemy07/Chinese-PDF-to-Text/blob/main/chinese_test_file.pdf).  You are also free to use your own, limit to 500 pages.  Make sure to place the PDF document in the same directory as the **tesserocr** files (should be the same as the directory that opens up when Jupyter Notebook opens).  To test, activate the appropriate virtual environment, and open up Jupyter Notebook.  Next, the PDF to text program will generally look like the code below:
```
from pdf2image import convert_from_path
from tesserocr import PyTessBaseAPI

pdf_file = <string of pdf file.pdf>

# Store all the pages of the PDF in a variable
pages = convert_from_path(pdf_file, 500) 
  
# Counter to store images of each page of PDF to image 
image_counter = 1
  
# Iterate through all the pages stored above 
for page in pages: 
  
    # Declaring filename for each page of PDF as JPG 
    # For each page, filename will be: 
    # PDF page 1 -> page_1.jpg 
    # PDF page 2 -> page_2.jpg 
    # PDF page 3 -> page_3.jpg 
    # .... 
    # PDF page n -> page_n.jpg 
    filename = "page_"+str(image_counter)+".jpg"
      
    # Save the image of the page in system 
    page.save(filename, 'JPEG')
    
    # print file name that is being analyzed
    print(filename)
    
    # Loads the recently made .jpg/jpeg file and converts image to text based on trained tesserocr dataset
    with PyTessBaseAPI(path=r<string that goes to the tesserocer trained database>, lang='chi_sim') as api:
        api.SetImageFile(filename)
        print(api.GetUTF8Text())
  
    # Increment the counter to update filename 
    image_counter = image_counter + 1
```


My input into Jupyter Notebook was: 
```
1 | from pdf2image import convert_from_path
2 | from tesserocr import PyTessBaseAPI
3 | 
4 | pdf_file = "chinese_test_file.pdf"
5 | 
6 | # Store all the pages of the PDF in a variable
7 | pages = convert_from_path(pdf_file, 500) 
8 |   
9 | # Counter to store images of each page of PDF to image 
10| image_counter = 1
11|   
12| # Iterate through all the pages stored above 
13| for page in pages: 
14|   
15|     # Declaring filename for each page of PDF as JPG 
16|     # For each page, filename will be: 
17|     # PDF page 1 -> page_1.jpg 
18|     # PDF page 2 -> page_2.jpg 
19|     # PDF page 3 -> page_3.jpg 
20|     # .... 
21|     # PDF page n -> page_n.jpg 
22|     filename = "page_"+str(image_counter)+".jpg"
23|       
24|     # Save the image of the page in system 
25|     page.save(filename, 'JPEG')
26|     
27|     print(filename)
28|     with PyTessBaseAPI(path=r'C:\Users\JohnnyC\tesserocr-master\tessdata', lang='chi_sim') as api:
29|         api.SetImageFile(filename)
30|         print(api.GetUTF8Text())
31|   
32|     # Increment the counter to update filename 
33|     image_counter = image_counter + 1
```
This will print out the entire PDF document. A shortened output was:
```
page_1.jpg
科技 成 果 与 知识 产权 1099

科学 技术 傈 黎 规 定

(1995 年 1 月 6 日 国家 科学 技术 委员 会 、 国 家 保密 局 发 布
国家 科学 技术 委员 会 、 国 家 保密 局 令 第 20 号 )

第 一 章 总 则

...

第 三 十 三 条 ”本 规定 由 国家 科 委 解释 。

第 三 十 四 条 ”本 规定 目 发 布 之 日 起 施行 。 经
国务 院 批准 ,一 九 八 一 年 颁布 的 4 科学 技术 保密
条 例 》 同 时 废止 。
```
Other useful resouces
=====================

1. https://stackoverflow.com/questions/59544006/multiple-versions-of-anaconda-python-installation

2. https://medium.com/better-programming/beginners-guide-to-tesseract-ocr-using-python-10ecbb426c3d

3. https://www.geeksforgeeks.org/python-reading-contents-of-pdf-using-ocr-optical-character-recognition/

4. https://www.geeksforgeeks.org/working-with-pdf-files-in-python/
