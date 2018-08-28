# Scripting with Python and Arcpy

This is a brief tutorial for how to get started scripting ArcGIS commands in Python using Arcpy. Arcpy is a proprietary Python module that allows ArcGIS toolbox commands to be implemented through simple Python commands. There are many advantages to using `arcpy`, rather than manually executing commands in ArcMap or using ArcMap's Model Builder. Unlike manually executing toolbox commands in ArcMap, scripting in Python provides a record of your work, which you can return to to edit or check your work. Model Builder also provides a record of your work; however, it comes with some disadvantages. Writing iterative loops in Model Builder is confusing, and it is not possible to write multiple nested loops. Python code is ideal for simply executing repeated commands using loops or, even better, parallelized operations across multiple computer cores. Finally, my experience is that ArcGIS commands executed using Python codes run much faster, and throw mysterious error messages far less often.

A disadvantage of running ArcGIS using Python and `arcpy` is that is can be a little tricky to set up. This tutorial will go through the steps to set up your machine to run Python scripts that can successfully lauch the proprietary arcpy module and any other Python modules that may be useful in your scripts.

## Prerequisites

1. ArcGIS must be installed on your computer. This tutorial will assume that you have ArcGIS 10.3.1 installed on your computer, but the tutorial should apply to other versions of ArcGIS as well. 
2. That's it! You don't need to download Python, since every installation of ArcGIS 10.3.1 already comes with an installation of Python 2.7. Confirm that ArcGIS 10.3.1 (or whatever version you're using) has installed Python to the folder `C:\Python27\ArcGIS10.3\` by confirming this folder exists, and that `python.exe` exists within it. If you are using a different version of ArcGIS, or if ArcGIS installed Python to a different folder, you may need to modify the instructions below to fit your directory.

You may have another copy of Python already installed on your computer, and this could create issues when you launch Python and attempt to import the `arcpy` module. In the tutorial, I'll explain what you should do to make sure that your computer is launching the ArcGIS installation of Python, rather than the previously installed version, so that you can successfully import the `arcpy` module.

## Launching the ArcGIS Python installation from the command line

We want to be able to run Python scripts that call in the `arcpy` module. Because `arcpy` is a proprietary Python module, it cannot just be downloaded from the internet and installed on your computer, and it will not run with any installation of Python. It will only run with the installation of Python you received with your installation of ArcGIS (10.3.1 in my case). Therefore, we need to show the computer where it can find this installation of ArcGIS, and make sure that it finds this installation before it finds any of the other Python installations you may have on your computer.

On your Windows 7 computer, navigate to `Control Panel>System and Security>System>Advanced System settings`. Once you open the System Properties window, select the "Environment Variables" button. If you have administrative privileges on your computer, select the variable "Path" under System variables. If you do not have administrative privileges on your computer, select the variable "PATH" under User variables. Without deleting directories currently found in the "Path" (or "PATH") variables, add the following directory to your path, separating it from other directories by a semicolon:
```
C:\Python27\ArcGIS10.3;
```
The Path variable provides a list of directories your computer will look in when you type a command in the command window. Now that you have added the location of ArcGIS's Python installation to your path, your computer will be able to find that installation when you attempt to launch Python in the Command Prompt window, or when you execute Python scripts.

To confirm this, open up your Command Prompt window and type `python`. After a moment, the Python interpreter should load in your command prompt window. Once it has loaded, type `import arcpy` and hit enter. After several seconds (arcpy is a fairly large module, so it takes a few seconds to load), Python will prompt you to enter another command, and your command prompt window should look like this:

![alt text](https://github.com/mwibbenmeyer/scripting_with_python_and_arcpy/blob/master/command_prompt_window.png "Logo Title Text 1") 

If that works, you're ready to start implementing ArcGIS toolbox commands using Python scripts! If you successfully loaded Python, but you received an error message after you typed `import arcpy`, the problem is likely that you have another installation of Python on your computer in addition to the installation provided by ArcGIS. So when you launch Python, your computer is launching the previously installed version, rather than the version provided with ArcGIS, which is capable of importing the arcpy module. To correct this, examine your path variable for another Python directory. If another Python directory is listed in your path, your computer is likely finding `python.exe` within that directory before it gets around to searching `C:\Python27\ArcGIS10.3`, and so it is loading the wrong installation of Python. To correct this, either delete the old Python directory from your path, or place it at the end of the list, after `C:\Python27\ArcGIS10.3`.

## Using arcpy with other Python modules

To make full use of arcpy and Python, you may want to use one of the many existing user-written Python modules in your Python scripts. pip is a command line program that is capable of installing Python modules from the internet. Navigate  here(https://pip.pypa.io/en/stable/installing/) and download `get-pip.py`, a script for installing pip, to `C:/Users/username/Downloads/`. Open the command prompt window and type `cd C:\Python27\ArcGIS10.3\`. Then execute the following command:
```
python C:/Users/username/Downloads/get-pip.py
```
The script will run and will install pip within your ArcGIS Python folder structure. To confirm this, use Windows Explorer to navigate to `C:\Python27\ArcGIS10.3\Scripts\`. You should find a variety of newly installed applications, including pip, wheel, and easy_install. 

Last, before using pip, you need to make sure your computer knows where it is. As we did before, we need to edit the path. Navigate to `Control Panel>System and Security>System>Advanced System settings`, click on "Environment variables", and add `C:\Python27\ArcGIS10.3\Scripts;` to your System "Path" variable or your User "PATH" variable, depending your level of administrative privileges.

Now, try installing a user-written module using pip. Try installing pandas, a popular data analysis module. Click "Okay" within the Environment Variables window, open a new Windows command prompt window, and type:
```
pip install pandas
```
If you are installing the package on your personal machine, you will (hopefully) receive a message that says that you have successfully installed pandas. If you are using a shared machine, you may not have privileges to install pandas into a system folder. A solution is to instead install the module to a user folder. Type:
```
pip install pandas --user
```
Now (hopefully), pandas should have installed in a user directory, and will be available for use in your Python/arcpy scripts. To confirm, type the following commands in a new command prompt window:
```
python
import arcpy
import pandas
```
If arcpy and pandas both import successfully, you're ready to begin writing scripts that integrate commands from both the arcpy and pandas modules. You also now know how to use pip to easily install any other user-written Python module you might want to use.

