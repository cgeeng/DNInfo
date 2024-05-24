# Domain Name Information - Instructions
The internet is made up of a series of protocols, one of the most notable protocols being the "Internet Protocol". This is the protocol that allows computers to communicate with each other over the internet. These are numbers often seen as 129.47.21.1 (as an example). However, the internet is not just a series of numbers, it is also a series of names. These names are called domain names. Domain names are human-readable names that are used to identify a website. For example, the domain name for Northeastern University is "northeastern.edu". To connect names to numbers, the internet uses a series of servers called Domain Name Servers (DNS). These servers are responsible for translating domain names into IP addresses, and combined with other servers that help with routing, allow you to connect to the website you want to visit.

For this assignment, we will explore keeping our own file of domain names and their associated IP addresses. This file will not only contain the domain name and IP address, but also the location of the server that sent the request. It is worth noting the web is a bit more complicated than this, as often a website like Northeastern, may have multiple servers throughout the world. This is done to help with speed and reliability. However, for our purposes, we will just assume one server and one location for our files. 



## Learning Objectives
* Practicing Test Driven Development
* Implementing junit tests for all methods
* Writing out in different file formats (CSV, JSON, XML, TXT)
* "Serializing" and "Deserializing" data (converting data to a file format and back)
* Using third party libraries to assist in serialization/deserialization (Jackson and/or Open CSV)
* Exploring network connections in java and using them to connect to a server
* Encapsulating data between components, making use of immutable and mutable structures where important


## Instructions

For this assignment, while we have provided example code we have written, including files you are welcome to use - you are free to fully design this project from the ground up as long as you:

* Use `DNInfoApp.java` as the main entry point of your program, and implement the argument options below. 

When designing your application, you want to make sure much of what you provide is immutable or unmodifiable, except for in the actual containing class as you pass information around to different components of your program. This is a good practice in software engineering, and one of the learning objectives of this assignment.  

The goal of the application is to provide Domain Name (URL) Information back to the client, based on the command line arguments provided. This information will be downloaded from the internet, and stored in a file. Once stored in a file, you do not have to update / download again (which is more common due to IP addresses shifting to balance load). As such, you are looking up the information in the file, downloading *and updating the file* if the domain name (hostname) is not found, and returning the information.  The records/local file will be kept as an XML file. 

Automatically updating a local file based on information obtained online is very common to prevent both unneeded web requests, and to make sure there are local copies. Most DNS servers and routing tables work in a similar way, though they are more complex and have mechanisms to continually update as locations change. To get information about the hostname, we are using a combination of built in java libraries and the [ipapi.co](https://ipapi.co/api/?java#introduction) API. This API is free to use up to a certain number of requests (1000 / day, 30,000 / month), so for use in this class you should be fine being in the free tier.  

### Display Formats

For displaying the information to the client, you will have to display it based on different formats provided, namely, the data can be displayed in a pretty print format (provided), or in a CSV, JSON, or XML format. For the JSON format, you should use the [Jackson Library](https://github.com/FasterXML/jackson?tab=readme-ov-file) to serialize and deserialize the data. For CSV, you can use the [Open CSV](https://opencsv.sourceforge.net/) library, Jackson, or write it using standard java (none of the data has commas in it, so simple formatting is fine). For XML, you can use Jackson, or write it out using the built in java libraries. We have already included the updates to your build.gradle, so the libraries are included. If you are curious, they are in the dependencies section of the [build.gradle](../build.gradle) file.

In addition to displaying the data to the stdout, you will also need to be able to write the data to a provided file. 

> [!NOTE]
> What does serialize / deserialize mean? It is a fancy way of saying "convert to a file format" and "convert from a file format". This is a common term in software engineering, and you will see it often. The file format can often be a binary file, but in this case we are using human readable text file formats. Often it is used when the class fields are bound to specific data in the output, so they can be easily converted back and forth.  


### Command Line Argument Options

The program should be able to take in the following command line arguments in any order. 

* `-h` or `--help` - Display the help menu and end the program, we have provided this menu for you in the `ArgsController.java` file.
* `-f` - The format to write out to. The options are `pretty`, `csv`, `json`, `xml`. If this is not provided, you should default to pretty.
* `-o` - The output file to write to. If this is not provided, you should write to `stdout` (System.out).
* `--data` - This sets the 'database' file to read from. If it is not provided, it is `data/hostrecords.xml`.  
* hostname | all - it assumes anything that doesn't have one of the above options is a hostname - just the first one that shows up. If `all` is typed in, it displays all contents of the file. If hostname is omitted it will display all contents of the file. 

You should try these command line arguments with the sample application provided, so you can get a better understanding of what should be displayed. 

> [!TIP]
> We have included a sample completed program you can try out. No promise that it is
> 100% bullet proof, and probably some typos, but you can get the idea
> of how the final program works. If you go into the sample_working directory using your terminal, you can run the program with the following command:
> ```console
> bin/DNInfo
>    or if windows
> bin\DNInfo.bat
> ```
> Both commands will execute the .jar file in the lib directory while properly setting the path. For macOS and linux, 
> you may need to add the execute permission to the file (`chmod +x bin/DNInfo`).
>
> This program uses *extensive* command line arguments (in fact it is only a command line program). Use -h to see the help menu.
> ```
> bin/DNInfo -h  
> ```



### What we provided

We have provided a number of files for you. You DO NOT have to use any of the files except for `DNInfoApp` as your starting point. You do not have to design it the same way. We just left some of our packages there to help you get started/get hints. With that said, there are definitely some components you will want to work with, and we encourage you running/modifying the couple test files provided to get a better understanding of the jackson library, along with the network utilities we provided. 

* `DNInfoApp.java` - The main entry point of your program. You need to keep this file, and start here for your program
* `controller/ArgsController.java` - A class to help you parse the command line arguments. We provided it, so you can have a premade help menu (saves copy and paste errors). 
* `model/DomainNameModel.java` - an interface for the domain name model. We made use of internal classes such as DNRecord as an example on how to use jackson annotations, and to pass information around the program, and also a 'getInstance()` method to get the singleton instance of the model. We left in the other features of the interface that we implemented, so you can see our interface *BUT* once again, you are free to design your own.
* `model/net/NetUtils.java` - Contains utilities for getting IP from hostname, and then getting the data from Ipapi.co API. You will probably want to use this file as is. To better understand this file, we have also included `test/TestNetUtils.java` which you can run to see how the file works, and edit to get a better idea of how NetUtils works. 
* `formatters/Formats.java` - Enum for the different formats.
* `formatters/DomainXMLWrapper.java` - If you use Jackson for XML, this class (or similar) is needed to get it formatted corrected
* `formatters/DataFormatter.java` - A class we used to help us format the data based on the provided format. Assumes a collection of DNRecords. We provided the 'pretty print' option, so you didn't have to worry about spacing or spelling with the autograder. You do not have to use this file, but make sure you follow the example pretty print format provided. You will want to look at `test/TestJacksonSerialize.java` to see how to use the Jackson library to serialize and deserialize data.


> [!CAUTION]
> These are not all the files we used! Don't be fooled thinking it is complete. We had files to help us especially in the model including the main model file, along with a java bean for loading data from online and more. Really think about design, and if you want to even use what we provided, but no matter what you will be adding files!  This assignment is intentionally open for your design, we just wanted to give you a jump start on some of the tougher topics that involved working with other libraries and network connections.

### :fire: Task 1: Design 

Before you start writing, it is important to think about design. You DO NOT have to be perfect in your design, so we will come back to this step a few times. 

1. First, become a detective and read through the files provided. Take notes on what you are seeing.  This is a common skill in software engineering, and you will need to do this often as you work with other people's code. [Report.md](../Report.md) has specific questions on the code that may help you. 
2. Go to [DesignDocument.md](../DesignDocument.md) and fill out (ONLY) the initial design sections. We have broken them up into sections to help you think about the design in smaller parts.

> [!TIP]
> You are free  to use mermaid or any other UML tools you want, just make sure if you are using another UML tool, you properly link the image in the markdown file. See the resources page, for a list of [UML tools](https://github.com/CS5004-khoury-lionelle/Resources?tab=readme-ov-file#uml-design-tools).


### :fire: Task 2: Implement by Test Driven Development

After your initial design, you should seek to follow the TDD process. This means you should write tests first, and then implement the code to pass those tests. Or better stated, you should write *ONE* test first, implement, and repeat until you have written all your tests. 

1. Figure out a number of tests by brainstorming (done in design)
2. Write **one** test
3. Write **just enough** code to make that test pass
4. Refactor/update  as you go along
5. Repeat steps 2-4 until you have all the tests passing/fully built program

Note: you often don't know all the tests as you write. As such, it is alright to continue to expand your list. This is where people get stuck on TDD. They think they have to know **all** the tests before they start. You don't. You just need to know the next test, and then at the end you double check you have covered all code paths and have full coverage. 

> [!CAUTION]
> Make sure to commit as you development. The bare minimum commits would be after every test, but you probably will have additional commits especially at the beginning. 

#### :raising_hand: Implementation Tips





> [!WARNING]
> If you modify one of our files, you need to add tests for it. If you do not modify a provided file, you don't need to add tests. This can be done without modifying our provided code, but you are welcome to modify it (except for the caveat about IGameList and IPlanner) if it fits your overall design better. 
 
### :fire: Task 3: Finish Design Document

By this point, your design has probably changed (very few people have perfect designs the first iteration). Update your design document with the final design in the "final design" section. We want to see the history of your first design to your final design. That is a good thing. 


### :fire: Task 4: Finish Report.md

Inside of [Report.md](../Report.md) you will need to answer a series of questions about your program, and about the learning objectives for the module in general. Fill it out. 


> [!IMPORTANT]
> A primary purpose of this activity is to get you working through a process in addition to writing code. In software engineering the process you follow is often just as important as the code you write. This is because the process is what allows you to work with others, and to be able to maintain and update code over time. It may seem tedious right now, but it is a skill that will pay off in the long run.




## Submission

When you are completed, you need to submit your code to gradescope. Go back to Canvas, and click through the link that takes you to the Gradescope assignment. When you submit, you will actually need to pull from your github repository in the dialog that appears. It only pulls the most recent submission, and if you make an update to the repository after you submit, you will need to resubmit to get the latest version in gradescope. 


## 📝 Grading Rubric

1. Learning (AG)
   * Code compiles without issue
   * Code passes all tests 
2. Approaching (AG)
   * Passes the style check.  
3. Meets (MG)
   * README.md is filled out (name, github repo, etc) 
      * With out the link to your repo, the TAs won't grade the rest!
   * DesignDocument (INITIAL) sections are filled out 
   * All methods are tested with JUnit tests
   * Method contain proper javadoc comments (not just javadoc notation but proper wording in the comment)
   * Report.md technical questions are questions answered correctly.
4. Exceeds (MG)
   * Code is DRY (Don't Repeat Yourself)
      * Including making use of helping/utility classes to reduce duplication.
   * Student uses proper inheritance without duplication 
   * Methods include tests for edge cases in addition to happy path
   * Proper use of sorts and sort strategy
   * Design document (FINAL) sections are filled out 
     * The notation needs to be correct, and the TAs will double check the final design
     matches the final implementation.
   * Report.md Deeper Thinking question filled out
     * Includes at least two references/citations

Legend:
* AG - Auto-graded
* MG - Manually graded

### Submission Reminder 🚨
For manually graded elements, we only guarantee time to submit for a regrade IF you submit by the DUE DATE. Submitting late may mean it isn't possible for the MG to be graded before the AVAILABLE BY DATE, removing any windows for you to resubmit in time. While it will be graded, it is always best to submit by the due date, so you have full opportunity to improve your grade.

If you need a reminder about the grading policy, please review the syllabus and the canvas page on 'formative/summative' grading. This class uses a unique grading system that will allow you to be flexible with due dates and multiple resubmissions (if you submit with time for TAs to give feedback), but we also ask that you continue to work on the assignment until you get a full grade.


### Autograder Limitation
Currently the autograder is limited in how it can test. As such, when it comes across an error it just stops. This means that if you have multiple errors in your code, you may only see the first one. We are working on improving this, but for now, you will need to fix the first error, and then rerun the tests to see the next error. Eventually, if every test passes, you will get the single point. It also may give you points for valid style, while errors exist in the code - so really assume the first 2 points are done together. 
