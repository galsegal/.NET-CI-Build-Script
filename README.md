#.Net Build Script For Continuous Integration 
Going forward to CD is a blessed improvement in a developer's life. The first step in this process is a the ability to build, test, package the code and send it to the CI orchestration tool (Jenkins, TeamCity etc.). This small repo aims to help in this step.

I have used [MsBuildTasks], an open source community effort to extend MsBuild capabilities.

##Before You Start

####CMD and MsBuild
Running MsBuild via cmd need some tweaks. You should add MsBuild reference to your PATH:

+ open the "Start Menu"
+ right click "Computer"
+ select "Properties"
+ in the Control Panel window click "Advanced system settings" on the bottom left side
+ click "Environment Variables"
+ under "System variables" look for "Path" and select it.
+ click the "Edit" button
+ add to the variable value the following line

```
    C:\Windows\Microsoft.NET\Framework\v4.0.30319;
```

> make sure you add the semicolon at the end of the line

####NUnit Dlls

You need to have NUnit installed on your computer. Just grab the MSI from [here][1].

##Adding This Script To Your Solution

####1. Add MsBuild Community Tasks Dlls

Use Nuget to add the build tasks:

```
PM> Install-Package MSBuildTasks
```

Your solution will be added with a new solution folder named ".build" with a dll and .targets file.

The important part is the "Build.proj" script which contains the build instructions for your project.

####2. Replace Build.proj with my Build.proj  file
####3. Update The Build.proj file with your specific data
In the new Build.proj file you will see a section called "PropertyGroup" at the beginning of the file. I tried to arrange all variables that needs change in this group. 

The most important thing you need to change is "ProjectName", which is the name of the folder where the executable or website files are. This project will eventually be zipped.

All the rest are just variables needed for the process and you don't need to change them:

+ "BuildPlatform" - 'Any CPU' should be enough
+ "Configuration" - 'Release' or 'Debug'
+ "VersionFile" is a text file that will be added to the root of the solution for managing version number. It will be incremented in every build.
+ "Artifacts" is a folder that will be added to the root of the solution and will contain the test results file and zipped file with your code for shipping.
+ "ProjectPath" - path to the project where the executable files are.
+ "TestResultsPath" - path to save test results xml.
+ "MSBuildCommunityTasksPath" - path to MsBuild community tasks dll and .targets file.


Below The "PropertyGroup" you modified there is an "ItemGroup". Update "NUnitConsole" value to match NUnit files path.

##Usage
The script support 3 targets:

**Build** - cleans all solution files and rebuilds:

```
MsBuild Build.proj /t:build
```

**Test** - builds and runs NUnit test runner:

```
MsBuild Build.proj /t:test
```

**Package** -  builds, tests, zips the executable and places it under "Artifacts" folder in the root of the solution:

```
MsBuild Build.proj /t:package
```

or just: (this is the default target, as can be found in the first line of the script)

```
MsBuild Build.proj
```

[MsBuildTasks]: https://github.com/loresoft/msbuildtasks
[1]: http://nunit.org/?p=download
