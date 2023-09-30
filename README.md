![image](https://user-images.githubusercontent.com/43964243/235778441-9dfb45ab-befd-480b-bc30-5eab5dc2efef.png)

# Unreal Engine Template Module

<!-- Header Start -->
<a href = "https://docs.unrealengine.com/5.3/en-US/"> <img height="40" img width="40" src="https://cdn.simpleicons.org/unrealengine/white"> </a> 
<a href = "https://learn.microsoft.com/en-us/cpp/c-language"> <img height="40" img width="40" src="https://cdn.simpleicons.org/c"> </a>
<a href = "https://learn.microsoft.com/en-us/cpp/cpp-language"> <img height="40" img width="40" src="https://cdn.simpleicons.org/c++"> </a>
<a href = "https://learn.microsoft.com/en-us/dotnet/csharp"> <img height="40" img width="40" src="https://cdn.simpleicons.org/csharp"> </a>
<img align="right" alt="stars badge" src="https://img.shields.io/github/stars/jdsherbert/unreal-engine-template-module"/>
<img align="right" alt="forks badge" src="https://img.shields.io/github/forks/jdsherbert/unreal-engine-template-module?label=Fork"/>
<img align="right" alt="Visitors" src="https://visitor-badge.glitch.me/badge?page_id=github.com/jdsherbert/unreal-engine-template-module"/>
<!-- Header End --> 

-----------------------------------------------------------------------

<a href="https://docs.unrealengine.com/5.3/en-US/"> 
  <img align="left" alt="Unreal Engine Template" src="https://img.shields.io/badge/Unreal%20Engine%20Template-black?style=for-the-badge&logo=unrealengine&logoColor=white&color=black&labelColor=black"> </a>
  
<a href="https://choosealicense.com/licenses/unlicense/"> 
  <img align="right" alt="License" src="https://img.shields.io/badge/License%20:%20Unlicense-black?style=for-the-badge&logo=unlicense&logoColor=white&color=black&labelColor=black"> </a>

<br></br>

-----------------------------------------------------------------------

## Overview
A repository containing a dummy Unreal Engine 5 gameplay module. Contains a folder structure that has been premade to help kickstart the making of new Unreal gameplay modules. I'm always forgetting what specifically I need to do, so this is a nice point of reference. Using this will allow you to create unreal engine modules for your project much more easily.

For more information I'd strongly recommend following Epic's gameplay module documentation, found here:
 - Full Documentation:  https://docs.unrealengine.com/5.3/en-US/gameplay-modules-in-unreal-engine/
 - Example (ModuleTest): https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/ProgrammingWithCPP/ModuleQuickStart/

For Coding standards, please see here:
 - https://docs.unrealengine.com/4.27/en-US/ProductionPipelines/DevelopmentSetup/CodingStandard/

If you'd like to get Unreal Engine and use this project template, please follow these steps:
1. Download the Epic launcher: https://www.unrealengine.com/en-US/
2. Install Unreal Engine (whichever version - this template is for 5.3)
3. Clone this project
4. Open it up with your engine, or copy + paste the module folder structure from it into your Source folder
5. Make sure to add the module to MyProject.Build.cs and MyProject.uproject

Lemme know if you have any questions or feedback!

  -----------------------------------------------------------------------

  # Guide

  -----------------------------------------------------------------------
  <a href="https://docs.unrealengine.com/5.2/en-US/"><img align="top" alt="Unreal Engine Guide" src="https://img.shields.io/badge/Unreal%20Engine%20Guide-00549F?style=for-the-badge&logo=unrealengine&logoColor=white&color=black&labelColor=midnight"></a>
  
  -----------------------------------------------------------------------
Unreal Engine Modules are a useful way to organize your code, improve your project's build times, and configure its loading and unloading processes for systems and code. This guide will cover the steps necessary to implement a new runtime module from scratch in C++. This module will be separate from the primary module for your project.

The module in this example is named ModuleTest.

1. Required Setup
To follow this guide, start by creating a new project named MyProject, and make sure it is a C++ project. If you are following along with a Blueprint-only project, create a new piece of C++ code in Unreal Editor to convert it to a C++ project.

2. Setting Up Your Directories
First, you need to set up directories to contain the module and its code.

Navigate to your project's root directory, then open the Source folder.

Inside your project's Source folder, make a new folder named ModuleTest. This will serve as your module's root directory.

Inside your module's root directory, create two new folders called Private and Public.

Your directory structure should resemble this:

- MyProject
  - Source
  - MyProject.Target.cs
    - MyProjectEditor.Target.cs
    - MyProject (Primary game module folder)
      - Private
      - Public
      - MyProject.Build.cs
      - Other C++ classes in your game module
    -  ModuleTest
        - Private
        - Public

3. Creating the Build.cs File
When Unreal Build Tool (UBT) looks through your project's directories for dependencies, it ignores your IDE solution file and instead looks at the Build.cs**** files in your Source folder. Every module must have a `Build`.cs file, or else UBT will not discover it.

To set up your Build.cs file, follow these steps:

Create a file called ModuleTest.Build.cs in your module's root directory. Open this file and add the following code:

### ModuleTest.Build.cs
```
using UnrealBuildTool; 

public class ModuleTest: ModuleRules 
{ 
    public ModuleTest(ReadOnlyTargetRules Target) : base(Target)
    {
    } 
}
```

This will make your module visible to the Unreal build system.
Edit the constructor so that it reads as follows:

### ModuleTest.Build.cs
```
public ModuleTest(ReadOnlyTargetRules Target) : base(Target) 
{ 
    PrivateDependencyModuleNames.AddRange(new string[] {"Core", "CoreUObject", "Engine"}); 
}
```
This will add the Core, CoreUObject, and Engine modules as private dependencies for your module. This will make several classes available to this module for later steps in this guide.

Now when you add code to this module's folders, that code will be discovered both when UBT builds the project and whenever you generate your IDE project files.

4. Implementing Your Module in C++
While the Build.cs file makes your module discoverable to UBT, your module also needs to be implemented as a C++ class so that the engine can load and unload it.

Fortunately, Unreal Engine includes macros that can streamline this process for most common implementations. To make a quick implementation file, follow these steps:

Navigate to your module's root directory and open the Private folder. Create a new file called ModuleTestModule.cpp inside this folder. It is not necessary to make a .h file for this class.

[ModuleName]Module is the typical naming convention for module implementation files in Unreal Engine source code. This is useful for keeping track of them in a large codebase.

Inside ModuleTestModule.cpp, add the following code:

### ModuleTestModule.cpp
```
#include "Modules/ModuleManager.h"

IMPLEMENT_MODULE(FDefaultModuleImpl, ModuleTest);
```

This provides a default implementation for your module that makes it available to C++ code.

You can create more detailed implementations by manually writing the class for your module, its constructor, and the Startup and Shutdown functions for it. However, for most gameplay modules, this default implementation will be sufficient to load and unload your module.

5. Compiling Your Module
After defining and implementing your module, the process to compile it is very simple.

Right-click MyProject.uproject and click Generate Visual Studio Files to regenerate your IDE solution.

This will ensure that your new module becomes visible in your IDE and within Unreal Editor.

Any time you re-organize your module's code or change the contents of its Build.cs file, you should regenerate your IDE solution to make sure it stays up to date.

Compile your project in Visual Studio. Your new module will be compiled alongside it.

Any time you add a new module manually, change your module's directory structure, move or rename C++ files, or change your module's dependencies, you should regenerate your project files to update your Visual Studio solution, then compile your project again.

Although you can compile your project while Unreal Editor is running, sometimes you will need to close Unreal Editor, then restart it after compiling for major changes or new classes to take effect.

6. Including Your Module In Your Project
Your module should be able to compile, but to use any code from your module in your project, you need to register it with your .uproject file, then make a dependency for your game's primary module.

Open the MyProject root directory, then open MyProject.uproject in a text editor and edit your "Modules" list as follows:

### MyProject.uproject
```
"Modules":
[
    {
        "Name": "MyProject",
        "Type": "Runtime",
        "LoadingPhase": "Default"
    },
    {
        "Name": "ModuleTest",
        "Type": "Runtime"
    }
]
```
You can use this list entry to configure which Loading Phase it will load up on and its Type. This example is a runtime module, so it can be used when the project is running as a standalone application or in the editor.

By comparison, Editor modules can only run in the editor. It also uses the Default loading phase, which initializes after game modules are loaded. Depending on the specifics of your module, you might need to use earlier loading phases to make sure other modules that depend on it don't throw errors looking for unloaded code.

Navigate to your MyProject/Source folder, then open the MyProject.Build.cs file. Add ModuleTest to your PublicDependencyModuleNames list. It should as follows:

### MyProject.Build.cs
```
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "ModuleTest" });
```
Now it will be possible to include code from your module in your primary game module.

7. Adding Code to Your Module
You can add C++ files to the Public and Private directories in your module manually, or you can add them in Unreal Editor.

These steps will guide you through using the New Class Wizard to add code to a module:

Open your project in Unreal Editor.

In the Content Browser, click Add, then click New C++ Class.

Choose Actor as the parent class, then click Next.

Locate the dropdown next to the Name field. It should read MyProject (Runtime) by default. Click this dropdown, then change it to ModuleTest (Runtime).

If you do not see MyModule (Runtime) as an option, review the previous sections to ensure you followed the steps correctly.

Name the class ModuleActorBase, set the Class Type to Public, then click Create Class.

Your class's .h and .cpp files will open automatically in your IDE. Your class's .cpp file will be added to your module's Private folder, while its .h file will be added to the Public folder.

Open ModuleActorBase.cpp, then add the following line to the AModuleActorBase::BeginPlay function:

`GEngine->AddOnScreenDebugMessage(0, 5.0f, FColor::Blue, TEXT("Hello, world!"));`

Save your code and compile your module.

This class defines a simple Actor that outputs an on-screen debug message when the game starts. You can test this by using the Place Actors tool in Unreal Editor.

8. Extending Code From Your Module
Finally, follow the steps below to test your new Actor class and your primary game module's ability to see it.

Open your project in Unreal Editor. Create a New Blueprint Class, then expand the All Classes list.

Choose ModuleActorBase as the parent class. Name your Blueprint class ModuleActorBP.

Drag a copy of ModuleActorBP from the Content Browser into your game's world. Click the Play button.

If you do not see ModuleActorBase in the list of classes, make sure its header is in the Public folder, that it has the BlueprintType UCLASS specifier, and that you have added the ModuleTest module to your project's dependencies.

## Final Result
When you click Play, your project will start playing in editor, and the on-screen debug message "Hello, world!" will display in blue text. This Blueprint-based Actor extends a class that is defined in a separate gameplay module. When you write modules in the future, you will now be prepared to create extensible classes as a foundation for gameplay-specific code.
