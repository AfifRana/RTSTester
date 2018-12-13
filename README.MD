# RTS Tester

**RTS Tester** is a Java tool for automatic regression test selection (RTS) testing on open source projects.

## Introduction

When developers maintain a software, they often create test suite on main functionalities and periodically regression test their build on test suite to make sure their changes do not produce errors. The simplest regression testing strategy, *retest all*, reruns every test case in the test suite. However this strategy can be very expensive as the scale of test suite and software grows. Here comes another approach, *regression test selection (RTS)*, reruns only a subset of the initial test suite. Studies show that a good selection tool could significantly reduce the testing time through skipping tests that are unaffected by the changes developers made. 

There are a lot of RTS tools available, RTS Tester integrate two most popular RTS tools and let developer easily experiment their project with RTS tools over hundreds of builds. RTS Tester also provide environment to let developer customize their own testing flow. Including integration of *Evosuite*, an unit test generation tool for maven projects.
### RTS Tools used:
1. **Ekstazi**: State-of-the-art dynamic class-level RTS tool.
http://ekstazi.org/
2. **STARTS**: Static class-level RTS tool.
https://github.com/TestingResearchIllinois/starts
## Getting Started

These instructions will get you a copy of the RTS Tester up and running on your local machine for development and testing purposes. 

### Prerequisites

Things you need to prepare before using RTS Tester.

```
1. A github open source project you want to test. 
2. Java JDK 8.
3. IDE (Optional but recommended Intellij)
```
Note: RTS Tester works on both single and multi module maven projects. However, projects that require specific configuration to compile may not work properly.

### Installing

To install RTS Tester, you need:
1. Download RTS Tester:
```
git clone <INSERT GITHUB URL>
```
2. Download all required libraries for the RTS Tester. Use an IDE will handle this step easily.
3. And that's it!

## Usage


### Run with commend line

The simplest way to run RTS Tool in commend line:
**Don't forget to include the external libraries if you only use command line.

```
java RTSTester <The number of evaluations> <Github URL>
```
e.g
```
java RTSTester 100 https://github.com/JodaOrg/joda-time.git
```
This will run test in jada-time project from the 100th oldest SHA(revision evaluated) to the most recent SHA and log tests selected for each SHA and corresponding runtime.
### Run with IDE

Open RTSTester.java under src.edu.illinois.yukangs2 directory, manually setting evaluationNum and repoURL. Then run main function.

```
public static void main(String[] args) throws ... {
	...
	evaluationNum = 100;
	repoURL = "https://github.com/JodaOrg/joda-time.git";
	runRTS();
}
```

## Customize Tests

* **Methods:**
Typically we use the following functions to run RTS tools on selected project.
```
runRTS();
```
If the user want to customize test flow, he/she can write their own test in main function, the main functions are: (able to run in any order)
1. ```readCSV()```: read .csv files to import tests, return List of ```buildInfo``` Object, default format in csv is:
```
project name, build id, event type, previous SHA, current SHA
```
2. ```runComm()```: run commend-line comment.
3. ```createDir()```: create new directory.
4. ```getHashes()```: get hashes/SHA value from commit history, return List of String.
5. ```patchAndTest()```: add RTS tool dependency to ```pom.xml```, run test and log result.
6. ```isMultiModule()```: check if the project is a multi-module maven project, return a Boolean.
7. ```addParent()```: add \<parent\> section in ```pom.xml``` to modify test module order.
8. ```patchEkstazi() ```: add *Ekstazi* dependency to ```pom.xml```.
9. ```patchSTARTS() ```: add *STARTS* dependency to ```pom.xml```.
10. ```patchEvosuite() ```: add *Evosuite* dependency to ```pom.xml```.
11. ```testAndLogResult()```:   run test and log result, return ```TestResult``` Object.
* **Classes:**
1. **BuildInfo**: contains data
```
String repoName; // Name of the repository
long buildId; // Build id
String eventType; // push or pull_request
String preSHA; // SHA hash for previous build
String curSHA; // SHA hash for current build
```
2. **TestResult**: contains data
```
int testRun; // Number of tests run
double duration; // time used the run
```
***Detailed explanation for each function are in the source file.***
### *Examples*:
1. If I want to compare two evaluations of a project, find out how many tests are selected and how much time saved by Ekstazi:
```
String name = "joda-time";
File parent = createDir();

```

## Development Plan
* Improve RTS Tester stability and compatibility.
* Make RTS Tester more robust with more projects.
* Integrate RTS Tester with more RTS tools.

<!---
## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 
--->
## Authors

**Yukang Shen** - *MCS* - University of Illinois Urbana-Champaign
* Email: yukangs2@illinois.edu
<!---
[PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.
## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
--->
## Acknowledgments

* Professor Darko Marinov's suggestion throughout the project.
* etc