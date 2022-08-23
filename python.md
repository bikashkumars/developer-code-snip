# Enabled Miniconda on Windows Powershell

First open Powershell as Administrator mode, don't forget to replace <user> with your system's username in following path. Then execute the command on powershell

```
powershell -ExecutionPolicy ByPass -NoExit -Command "& 'C:\Users\<user>\Miniconda3\shell\condabin\conda-hook.ps1' ; conda activate 'C:\Users\<user>\Miniconda3' "
conda activate <your-conda-env-name>
```

# File handling
  
## import glob

Using glob package, you can search files and the end result will return you python list
  
```
path = "C:\\Users\\folderName"
productInfoFileArr = glob.glob(path + "/**/pom.yml", recursive = True)
```
  
## import os.path
  
Get Path name after excluding file name from path ( Or get the path to get the directory containing file )

```
absolutePath = os.path.dirname(filePath)
```
  
Get True or False - based desire directory present or not
  
```
os.path.isdir("directory/path/")
```
  
Get True or False - based desire path present or not
  
```
os.path.isfile("directory/path/pom.xml")
```
