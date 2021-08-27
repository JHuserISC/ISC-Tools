# ISC-Tools
InterSystems Tools and Utilities

# Table of Contents
* [Export Studio Custom Snippets to Visual Studio Code](#exportsnippets)
  * [Goal of this code](#goal)
  * [How-To run this code](#runcode)
    * [Prerequisites](#prerequisites)
    * [Install the Cache Object Script class](#installcode)
    * [Check Studio Snippet Code filename](#checkstudio)
	* [Check VSCode Snippet Code filename](#checkvscode)
    * [Run the command](#run)
  * [Use VSCode Snippets](#usevscode)
  * [How it works](#howitworks)
  * [ToDos](#todo)

<div id='exportsnippets'/>

# Export Studio Custom Snippets to Visual Studio Code

![main](https://raw.githubusercontent.com/JHuserISC/ISC-Tools/5d6ad2d63bab017cc134748f193735bba99bc28e/img/jpg/Studio_To_VSCode_Snippets.jpg)

<div id='goal'/>

## Goal of this code

The objective is to export User Defined Snippet Code from Studio to a JSON Format file that can be imported to VS Code User Defined Snippets.

The Code Snippets from Studio are exported as is, spaces indentation are exported as space, tabulation indentation are exported as tabulation.

Carriage Return are also exported. Make sure your Code Snippets are correct in Studio so that the JSON file will be correct too.

<div id='runcode'/>

## How-To run this code 

<div id='prerequisites'/>

### Prerequisites

 * Studio 2020
 
 * Visual Studio Code

<div id='installcode'/>

### Install the Cache Object Script class

Either create a new class called Tools.CodeSnippets, copy and pass the content of the GitHub file Tools.CodeSnippets.cls and compile.

or import XML Tools.CodeSnippets.xml from GitHub source.

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/StudioClassCompiled.jpg)

<div id='checkstudio'/>

## Check Studio Snippet Code filename

Open Studio

Go to Tools >> Options >> Environment >> Code Snippets

Check the User Defined Snippet Code path and file name (Click on Edit from the Code Snippet files you want to export)

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/StudioCodeSnippets.jpg)

<div id='checkvscode'/>

### Check VSCode Snippet Code filename

Open Visual Studio Code

Go to File >> Preferences >> User Snippets

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/VSCodeSnippets1.jpg)

Select the snippets languages, i.e. objectscript (ObjectScript) language

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/VSCodeSnippets2.jpg)

Check the path and file name of the JSON files that should contains the Snippets.

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/VSCodeSnippets3.jpg)


<div id='run'/>

### Run the command

The command line is :
```objectscript
do ##class(Tools.CodeSnippets).ConvertCodeSnippet(strINStudioFile, strINVSCodeJSONFile, strINLanguages)
```
the input parameters are not mandatory and can be empty:
 * **strINStudioFile** : path and file name of Studio's Code Snippets text file, if empty, it will prompt a question which answer is mandatory
 * **strINVSCodeJSONFile** : path and file name of VS Code's JSON file to write, if empty, it will prompt a question to generate random file name on default temporary path or fill the output
 * **strINLanguages** : if empty all Snippets are exported, if the input parameters contains a list of ids separated by comma it will export only the snippets of those languages

Languages are:
 * 1 : IRIS Object Script
 * 2 : SQL
 * 3 : Class Definition Language
 * 4 : IRIS Basic
 * 5 : HTML
 * 6 : Unknown
 * 7 : User-Defined
 * 9 : XML
 * 11 : Javascript
 * 12 : IRISMVBasic
 * 13 : Java
 * 14 : TSQL
 * 15 : CSS
 * 255 : SQL Triggers

Open a IRIS Terminal (or user Studio Output), and execute the command line.

Example to use with a prompt asking for the files paths and names call and export all snippets: 
```objectscript
do ##class(Tools.CodeSnippets).ConvertCodeSnippet()
```
![main](https://github.com/JHuserISC/ISC-Tools/blob/f93aed88fbcb4c47e45741c3730544c69892eb36/img/jpg/Terminal2.jpg)

To use with the files path and names input as parameters call : 
```objectscript
do ##class(Tools.CodeSnippets).ConvertCodeSnippet("C:\Users\jhuser\Documents\InterSystems\CodeSnippets.txt","C:\Users\jhuser\AppData\Roaming\Code\User\snippets\objectscript.json")
```

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/Terminal.jpg)

To use with the files path and names input as parameters call : 
```objectscript
do ##class(Tools.CodeSnippets).ConvertCodeSnippet("C:\Users\jhuser\Documents\InterSystems\CodeSnippets2.txt","C:\Users\jhuser\AppData\Roaming\Code\User\snippets\java.json", "7,14")
```
![main](https://github.com/JHuserISC/ISC-Tools/blob/f93aed88fbcb4c47e45741c3730544c69892eb36/img/jpg/Terminal3.jpg)

<div id='usevscode'/>

## Use VSCode Snippets

Open Visual Studio Code and either check the content of JSON File within VS Code or try to type a snippet prefix.

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/VSCodeSnippetsLoaded.jpg)

<div id='howitworks'/>

## How it works

Studio Code Snippets are saved in a Text File. Each Snippets are separated by an invisible character : $char(1)

Each Snippet contains : a Name separated with $char(2) from the language of the snippet separated with $char(2) with the content of the snippet's code.

i.e. "Snippet Name" _ $char(2) _ "Snippet Language" _ $char(2) _ "Snippet Code Content" _ $char(1)

![main](https://github.com/JHuserISC/ISC-Tools/blob/f93aed88fbcb4c47e45741c3730544c69892eb36/img/jpg/StudioCodeSnippetFile.jpg)

The VS Code JSON Snippet files required for each Snippet a Name and attributes : prefix, body, description (optional)

from studio only Name and Code content are available. 

Description is set to the value of the Name.

Prefix is the value of the name with a conversion from alphaup to lower case.

![main](https://github.com/JHuserISC/ISC-Tools/blob/4c11c1e50b29cb059993d5723e6d0c9cd49d3855/img/jpg/JSONVSCodeFile.jpg)


<div id='ToDo'/>

## ToDo

- [ ] add more code comments
- [ ] Add options to convert spaces into indentation (tab)
- [ ] Create a GUI to execute the command line
