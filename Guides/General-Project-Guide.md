# General-Project-Guide

---
# Table of Contents
- [Setting Up Your Programming Environment](#Setting-Up-Your-Programming-Environment)
- [Project Requirements](#Project-Requirements)
- [Project Resources](#Project-Resources)
	- [Labs](#Labs)
	- [Project Guide](#Project-Guide)
	- [TA Help Videos and Slides](#TA-Help-Videos-and-Slides)
	- [Example Input and Output Files](#Example-Input-and-Output-Files)
	- [Pass-off Cases](#Pass-off-Cases)
	- [Other Resources](#Other-Resources)
- [Testing Your Code](#Testing-Your-Code)
- [Passing Off A Project](#Passing-Off-A-Project)

---

This semester, you will complete several coding projects. Each of them will build on the functionality of the previous ones until you have a complete, functional, and efficient Datalog interpreter.

This guide is intended to consolidate information concerning these projects: how to get your programming environment set up, general project requirements, and the resources available to help you. Please be sure to read the entirety of this document, and you may find it useful to reference it later to find answers to your questions.

## Setting Up Your Programming Environment

Follow the [Lab0 Instructions](/Labs/Lab0.md)

## Project Requirements

Each project has a specification posted on Learning Suite. The specification talks about the requirements your code should meet and provides example input and output. Many of the project specifications include sections describing implementation requirements and implementation suggestions. These sections can help you design and organize your code.

There are some requirements that are true for all the projects throughout the semester:

-   Projects must be completed individually (not by groups of students)
    
-   Projects must be passed off on the submission website or with a TA to be given credit
    
-   All output should be sent to stdout rather than a file
    

As you design and code your projects, be aware that your end product will be a complex program with lots of parts to keep track of. Using good object-oriented design principles by creating appropriate classes for different parts of your program and limiting dependencies between classes will benefit you in the long run. Following the implementation requirements and recommendations as well as the project guides and TA slides will help you with this.

## Project Resources

For each project, there are resources provided to help you succeed as you begin to design and code. This section provides an overview of each resource, including information on how to access it and what it is intended to help you with.

### Labs
For each project we have a corresponding lab. Please attend the lab session where a TA will review the instructions and answer questions. Complete this **before** starting on the project.

### Project Guide

There is a project guide for each project that is designed to supplement the project specification. They break the projects into manageable steps, give insight into how you might design and implement your program, and provide handy pseudocode. These [guides](/Guides) are PDFs available on Github. You can access them using the link in the project specification.

### TA Help Videos and Slides

The TA slides and help videos are there to help you further understand the projects. They include descriptions of the different classes you might include in a project and their purposes. They also address details that students often have questions about. You can access the help videos and slides as well by clicking on the appropriate tab underneath the project name on LearningSuite.

### Example Input and Output Files

The example input and output files will help you test your code and answer any questions you have about input and output that are not answered in the specification. If you are ever uncertain about how a particular edge case should be handled, you can look for example input and output that addresses it. Additionally, these test cases generally serve as a good indicator for whether or not your code will pass the pass-off tests. However, these test cases are not comprehensive! If all of these files pass, it does not guarantee that your program will perfectly pass the pass-off driver. Also write your own test cases; see if you can put something in that gets an incorrect output.

The example input and output for the first project comes with a makefile that allows you to automatically run all the test cases. Instructions for using the makefile are included on LearningSuite. This makefile can be easily modified to work for the other projects. You can find the makefile and the input and output files by clicking on the appropriate tab underneath the project name on LearningSuite.

### Pass-off Cases

The pass-off cases, if provided, will also help you test your code and answer questions about input and output that are not answered in the specification. These pass-off cases are almost identical to the ones that are used to test your code when you go to pass-off the project. The only difference is that there are copyright comments included at the top of the files available to you that are not included at the top of the pass-off cases used to pass-off.

Similarly to the example input and output files, there is a makefile provided for each project that will automatically run all the test cases. You can find the makefile and the pass-off cases by clicking on the appropriate tab underneath the project name on LeaningSuite.

### Other Resources

If, after using these resources, you still have a question or you find yourself stuck, please reach out to someone for help. You can post on Slack, ask a TA, or reach out to your professor.

## Testing Your Code

It is best to test your code incrementally. Write the smallest amount of code testable and then test it before adding more code. This process helps debugging go faster because you can more easily pinpoint where your bugs are.

Before you submit your project, test your code on the lab machines against all of the test cases provided and any that you have created. Testing on your own machine is not sufficient because code will commonly work a little bit differently on the lab machines than on other machines.

If your IDE is connected to the lab machines as recommended, you can do this testing manually but it may be time consuming. In addition, if you are inspecting your output files by eye it can be easy to miss a mistake. It is more convenient and less error-prone to use the provided makefiles.

To use these makefiles on the lab machines, you will need to complete the following steps:

1.  Copy the test files, makefile, and your project code to the lab machines
    
2.  Use a terminal to ssh into the lab machines and then compile and run your code
    


When testing your code, be sure that the command you are using to compile it is as follows:

g++ -Wall -Werror -std=c++17 -g *.cpp -o nameOfExecutable

Replace “nameOfExecutable” with whatever you want your executable to be named. Generally, the provided makefile will run this command for you if you type “make compile” into the terminal. Then, you can run the tests if you type “make”. 

If you want to run your program with a single file, you can use this command:

./nameOfExecutable fileName.txt

Be sure to replace “nameOfExecutable” and “fileName” with the appropriate names.

The makefile will easily and quickly run all the tests at once. Additionally, your output is compared to the expected output using a diff command, and any differences are reported so that you can be sure you don’t miss any mistakes.

If you find any errors while testing on the lab machines with the makefile, note the test case that failed and what the bug was. Then, debug using an IDE that is connected to the lab machines. You can also use GDB to debug from the command line, but this is a much more tedious process.

## Passing Off A Project

Once you have tested your code and ensured that it works, you are ready to pass off. To pass off, you will first need to create a zip folder that contains all of your .h and .cpp files. You can do this by selecting the files in your file explorer and compressing them from there. You will need to hold down your Ctrl key while selecting each file, then right click one of the selected files and select the appropriate option to zip the files (it might be something like “Send to” or “Compress”). Be sure to include all your code in the zip. Additionally, do not have a folder nested inside the zip. In other words, do not zip the folder that contains your code; zip the code files themselves.

Once you have a zip file, navigate to [https://cs236.cs.byu.edu/](https://cs236.cs.byu.edu/) upload your file to pass off the project. Your results will be shown in the response text. Note that we do not check for memory leaks in your code and they will not affect your score.