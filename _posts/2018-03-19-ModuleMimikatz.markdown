---
layout: post
title:  "How to Add a Module in Mimikatz?"
author: Little Prince
date:   2018-03-18
category: security
keywords: security and software
abstract: How can we define our module in mimikatz :)
---

Hi guys !
I am going today to write about "how can I add to mimikatz a module/s?"
Firstly, you must clone or download “https://github.com/gentilkiwi/mimikatz.git " mimikatz project" in github. I used Visual Studio Community 2015 to open the project.

We must try to build the mimikatz project before trying to add new files/modules. If it fails here, you must fix your development environment first before next operations.
I tried it and it worked. Let’s continue …
We create our own modular header and C file in /mimikatz/modules, here :
* /mimikatz/modules/kuhl_m_littlePrince.c
* /mimikatz/modules/kuhl_m_littlePrince.h

<!--!header(/assets/img/lp.jpg)-->
<p align="center">
  <img src="/assets/img/lp.jpg">
</p>

I will talk about some global variables that are used when you examine the project. Knowing what they are used for will help us in developing the module. My global variables **_kuhl_m_littlePrince_**  &  **_kuhl_m_c_littlePrince_** .

These types of global variables : **KUHL_M** and **KUHL_M_C**.

**_<font color="#DC143C;">“KUHL_M”  means Kiwi Userland High Level Module.</font>_**

KUHL_M is a struct that was defined by _"struct _KUHL_M"_ in kuhl_m.h.
```
typedef struct _KUHL_M {
	const wchar_t * shortName;  
	const wchar_t * fullName;
	const wchar_t * description;
	const unsigned short nbCommands;
	const KUHL_M_C * commands;
	const PKUHL_M_C_FUNC_INIT pInit;
	const PKUHL_M_C_FUNC_INIT pClean;
} KUHL_M, *PKUHL_M;

```

**struct KUHL_M** or  **\*PKUHL_M**  defined in **kuhl_m.h**. The parameters of this structure;

* shortName uses as module name,
* fullName uses for the display name listed
* decription uses for description of module
* nbCommands uses for **n**um**b**er of **C**ommands.
* commands uses for list of module functions that is defined in KUL_M_C
* As “PKUHL_M_C_FUNC_INIT”  defined name is refer to NTSTATUS. pInit and pClean are  variables type of NTSTATUS.



_pInit & pClean functions are not mandatory, only to initialize the module before calling functions, then when the module is unloaded
the prototype is the same for pInit & pClean: NTSTATUS kuhl_m_modulename_init/clean()_

**_<font color="#14866d;">“KUHL_M_C”  means Kiwi Userland High Level Module Commands.</font>_**

wchar_t is “unsigned short” .
PKUHL_M_C_FUNC defined a function of type “NTSTATUS”

```
typedef struct _KUHL_M_C {
	const PKUHL_M_C_FUNC pCommand;
	const wchar_t * command;
	const wchar_t * description;
} KUHL_M_C, *PKUHL_M_C;

```
* pCommand is our module function
* “command”  says us  how to call this module in terminal
* description  is a brief description of what the function does

The NTSTATUS type is defined in Ntdef.h, and system-supplied status codes are defined in Ntstatus.h.

_NTSTATUS values are used to communicate system information. They are of four types: success values, information values, warnings, and error values..._

_<font color="#D2691E;">NTSTATUS kuhl_m_modulename_functioname(int argc, wchar_t * argv[]);</font>_ it can return what it wants in a NTSTATUS , except: STATUS_FATAL_APP_EXIT, needed to exit mimikatz ;))

<p align="center">
  <img src="/assets/img/lp-2.jpg">
</p>


We have made the necessary declarations in our header file and passed the main file.

<p align="center">
  <img src="/assets/img/lp-3.jpg">
</p>

We created our module with kuhl_m_modulename.h & kuhl_m_modulename.c. We have one last step :)

We need to add the module in the mimikatz.h then the global variable in the mimikatz.c modules list.

<p align="center">
  <img src="/assets/img/lp-4.jpg">
</p>

<p align="center">
  <img src="/assets/img/lp-last.JPG"><br/><br/>
  Modules List
  <img src="/assets/img/modules.JPG">
</p>

You can now build the mimikatz, then run it !

Then you succeed!! Now you can see the message of the little prince :)  you are lucky :)

<p align="center">
  <img src="/assets/img/lp-6.jpg">
</p>

Have a good day ^^
