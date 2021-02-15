# How to deal with bugs

A little intro to the art of bug fixing and the etiquette of bug reporting, for the hackathon and for everyday work in the lab. As there is no precise workflow to follow, this is just a collection of sparse notes that are useful when dealing with lab custom code, prone to bugs.
Unfortunately, although code running is 99.9% deterministic, code writing is not, and there is no general schema or a precise debugging algorithm that always works. There is some experience and pattern recognition involved; most importantly, a good amount of patience is paramount. Take it as a crossword to be solved! In almost all cases, a solution will be found, and it will be a reasonable one.

For those reasons, the points below have some sort of order, but they are actually more like a collection of sparse tips. The only hard constrains are for the final section - how to get help - which you should read only after everything else!

First, some (potentially unprecise, but understandable) explanation of useful words and concepts:
 - **IDE**: [Integrated Development Environment](https://it.wikipedia.org/wiki/Integrated_development_environment), basically a text editor with integrated helper tools to write and run code (eg, PyCharm)
 - **real-time program**: programs like Stytra or Sashimi, where you have a GUI and an event loop that keep executing some Python code to make things happen in real time (aquire frames, show stimuli, react to mouse/keyword clicks, etc). Opposed to analysis libraries and scripts like bouter and fimpy, where lines of code are executed only once, or a fixed number of times.
 - **DLL**: [Dynamic-Link Library](https://en.wikipedia.org/wiki/Dynamic-link_library), compiled code (eg, in the `C/C++` language) that is somewhere in windows and can be loaded and used from a Python file

Let's start!

---

## 0. Don't panic
Computers are not mean. If something is not working, don't freeze and don't think that you can't understand what is happening! You can do it :)
Think rationally about the bug that you are facing and which of the following points you need to consider for understanding the problem and its solution.

---

## 1. Isolate the problematic line
Isolate the problematic line at which your code is failing. This is the essential point for all the subsequent tips. Learn to read carefully the error messages from Python and follow the list of calls that failed. You can't always expect the error message to be informative enough to solve the problem immediately; but you always have to read it carefully to trace the problematic line back. This is an essential skill for Python programming! You can find some intro [here](https://realpython.com/python-traceback/).
Print statements (or more fancily, PyCharm debug mode) can help a lot in the process, in the case of real-time programs with a complicated flow (eg, Stytra/Sashimi etc). Us them _ad libitum_!

Many times, reconstructing carefully the line that is raising the error is enough to think with a good degree of precision about what that error might be.

DLL not found, unspecified related segmentation faults and other errors that are not thrown from Python are the only really nasty things to isolate, as you might not find find the problematic line. For those cases, be ready to use even more patience, and keep reading :)

---

## 2. Replicate the problem!
Make sure the problem is reproducible! Many times, a very important step toward the solution of a bug is making sure we know at least the exact circumstances under which it happens.

### 2.0 Know your Python version and libraries
Make sure you know what Python is giving the troubles. With anaconda multiple environments you can have different Python executables in different environments. It is good practice when you code and debug to know [where in your computer is the python.exe file that is running](https://docs.anaconda.com/anaconda/user-guide/tasks/integration/python-path). It is also good to be aware of [how to get the version of a package](https://note.nkmk.me/en/python-package-version/) and [how to find source code of the files of the libraries you are using](https://stackoverflow.com/questions/31003994/anaconda-site-packages).

### 2.1 Replicate from terminal
Sometimes, execution of code from an IDE like PyCharm or Jupyter could give problems related to how those IDEs index your Python libraries (this happens in particular for import/DLL errors). The most deterministic way of running code is from the terminal (Anaconda terminal, it should be there on all our computers). With the terminal:
- you can be sure it is a Python bug and nothing related to the IDE;
- you can be sure about which python environment you are using; 
- you can give precise instructions for replication when asking for help.

### 2.2 If possible, simplify the situation
One of the most effective strategies of debugging is trying to replicate the bug in a simplified situation. Once you have found the line that is problematic, you can try ways of reproducing it in a small [PyCharm scratch file](https://www.jetbrains.com/help/pycharm/scratches.html), or copypasting code in an `ipython` ression running from the terminal (in the same environment!). This is fundamental, and easy to do for errors related to `import` statements, or hardware initialization/opening. It could be more difficoult for errors related to real-time execution of programs; for those, sometimes is a sequence of actions (eg, press some buttons in some order). The concept is still valid, try to isolate the minimal sequence of actions that triggers the error!

---

## 3. Understand precisely what is being executed when the program crashes
Once we figured out the problematic line, and a reproducible way of running it again and make it crash, we need to make sure we know everything happening at that line. If a function/method is running with some arguments, we need to know what value those arguments have (PyCharm debugging mode or print statements are your best friends there!). If it is a function that calls other functions inside, we need to make sure we explore all the nested calls to know exactly where the problem lies.

For real-time applications, figuring this out might be trickier, as functions can be called tens/hundreds of times per second, with different values. Again, meaningful print statements, used thoroughly, are the best way to investigate. Keep in mind that print never fails; if it does not print, your line of code is never reached, there is no other explanation!


**NOTE: the event loop**

To understand what is happening in real-time applications, you need to know they have an event loop: think about it as a `while program_is_on` statement that repeatedly executes lines of code non-stop until a program is closed. In most of our code this can be literally a `while program_is_on` loop, where your line hap (many times, somewhere in classes called `SomethingSomethingProcess`). Other times, if there is a GUI, it can ben a function/method triggered by a `QTimer` (in which case, there is somewhere a `QTimer.timeout.connect(your_problematic_function_or_its_caller)`)


---

## 4. Figure out what in the line is problematic
By this point, you should have all the points to understand the ultimate cause of your bug. Sometimes, this is enough to grasp what happens. Some other times, you might need to dig more (eg, the program is failing because of a variable that is left to `None`). For those scenarios, you need to keep following upstream the river of troubles until you find the root source that can be fixed!

---

## 5. Google it!
If you still don't understand, make sure you fully investigate it as much as possible on Google. [StackExchange](https://stackexchange.com) has answers to nearly any Python problem, and is very nicely indexed by google. If you have a problem that pertains a specific Python library, try to find it in GitHub (you can use [PyPI](https://pypi.org), the database `pip` installs from, which always has a link to the library repo; and explore issues there to see if anyone already had the same problem.

---

## 6. Sparse additional tips
Finally, here are some other points in random order of things you might want to consider when debugging!

### 6.0 Beware `try-except` clauses
`try-except` is an important Python construct [you must be familiar with](https://www.w3schools.com/python/python_try_except.asp). It is very convenient when developing; but it can be a pain when it comes to debug. You can find them often in complex real time application like stytra; make sure you know if the fragment you are debugging is in such statement, and that you understand the principle of its execution flow. For debugging, consider temporarily disable try-excepts. They can be particularly evil when variables are assigned a value inside them. You might need to use the power of the print statement in its full glory to understand what is happening in such scenarios.

### 6.1 Hardware control tips
Some tips for situations when python is interacting with external hardware such as cameras or NI boards. Harware related problems are always very easy to identify by the error they trigger (eg XimeaCamera error). For programs that have been tested thoroughly like Stytra or Sashimi in the near future, you can generally guess your problems happen at the level of harware initialization, for which the following points apply.

- **Make sure your hardware control is on**. As easy as it sounds.
- **Close other applications/ kill rogue processes**. Sometimes an opened program can prevent the hardware to be reached by Python (eg a camera viewer like SpinView, or NI MAX). Make sure all such applications are closed. This can happen also if you have rogue Python processes opened (usually because of a previous program crash). Make sure you kill from the Windows Terminal/Activity Monitor all applications called something like "python"
- **Make sure all requirements drivers and dependencies are there**. If your problem is raised when initializing some harware, it might be that a library is not installed. Eg, Stytra won't install by default all libraries for all possible cameras, and you need to make sure you have [installed what you need](http://www.portugueslab.com/stytra/userguide/0_install_guide.html#installing-camera-apis). Conceptually there are 2 parts of harware installation, although in some cases they can happen simultaneously in the installation procedure: 0. driver installation  and 1. python wrapper library installation. 
 
 Point 1. will generally simply rise an `import` error, solvable with a pip installation. But if you keep having funny hardware-related problems, it is paramount to make sure that point 0. has succesfully happened. Before keep crashing your head on Python, make sure that from some other software (eg Ximea CamTool, SpinView, NI MAX, CoboltMonitor, etc) you can access the harware without any problem! Will spare you a lot of time.

### 6.2 Numba bugs
Sometimes issues can arise from numba-ised functions (they have the `@jit` decorator before their definition). In those functions, Python code is compiled in C for performance purposes. As a result, you will see `numba`-related errors instead of the clean Python ones. In those cases, check
- what arguments you are passing to the function, as wrong arguments can result in uninformative errors
- if the error persist after commenting out the `@jit` decorator. If it does, it is likely to be a bug of the code in the function, or somethong related to the passed values. If it vanishes, it might be related to arguments problems that Python can deal with, and C can't (eg, variable types). Try to find arguments with which the function works, and make sure that what you are using matches those types. Finally, in a minority of case, if it vanishes after removing the decorator can be a `numba`-version related problem, solvable with `pip` installing a different numba version. If the most recent version of the package is failing with the most recent version of numba, you should report it to the developers.

### 6.3 Parallelization bugs
Sometimes, parallelized code (eg in `fimpy`) can fail in funny ways. When debugging a parallelized function, first make sure everything is running smoothly when calling it with a number of processes set to 1. If that is failing, you have a basic problem in your function; if it is not, try to print your way out of understanding what is going wrong when parallelizing. Keep in mind that parallel task might be highly memory and CPU intensive! Consider reduce the number of jobs or the size of the chunks they work with to solve memory related problems, or the computer freezing.

### 6.4 Recognise nasty bugs
Python is magic but it can break in funny ways. Try to develop a taste for really nasty problems; most of the times, they have to do with dependencies of the most core packages of python, DLL problems, binaries not found, and similar things. Be 100% sure: sometimes the ones that look the nastiest just come from little things like wrong conda environment or hardware not on!

But if you are, always consider the giant red button labelled `nuke my Anaconda and install it again`.

---

## 7 Report/ask for help
If all of the above has lead to nothing after some amount of time (reasonably: 0.5-2 hours are a reasonable time investment in the opinion of yours truly, variable depending on your hurry to fix something vs willingness to learn something), consider asking for help. When you do, please stick to those rules! They are very valid when asking someone in the lab, and when using StackOverflow/GitHub issues too.

0. Make sure you are aware of everything that has been written in the points above;
1. Give precise information about exactly what error is raised;
2. Describe a precise way of replicating the error;
3. Explain eventual steps that you have alredy taken in trying to understand what is happening. You don't have to describe everything, but  more is always better than less.
4. (for reporting  StackOverflow/GitHub issues) describe concisely but completely your setup: OS, Python  version, relevant libraries versions.


**NOTE**

Keep in mind that almost no developer will be annoyed by people asking questions and help on code, inside and outside the lab! Requests are always treated with respect, as long they are respectful themselves and as long as they show the efforts that you have already made in solving the bug. 

Make sure that your understandable frustration does not get in the way of effective communication!

**Don't:**

```
Python/numpy/Stytra does not work! Can you help me?
``` 

This is a trigger statement that will always kill from the start the good will of who you are asking to have a look into your problem!

**Do**

```
I have a problem with a camera in Stytra. 
I run it from the stytra_env environment and the screen stays black. 
Python does not raise any error when running Stytra from the terminal.
I can open the camera with the camera viewer. 
What can I do?
```

You have cleanly and concisely reported a problem, with a reasonable amount of detail for someone in the lab; it is perfectly fine if you don't know how to deal with it. Whoever you're asking will be happy to help you!

---

## 7.0 Use GitHub issues as much as possible!
GitHub issues are a very useful way to make sure that you document your issue to developers of a package, and even more importantly, to other users who might have the same problem. You always have to keep in mind that for a developer, receiving an issue is never taken as a personal critique, and is always welcomed as a (public) sign of interest for their code! So use them, as much as possible, both internally in the lab, and externally for packages out there! Only small note, make sure you browse the existing issues in the repo to see whether something similar was already raised.

### 7.0.0 GitHub issues in the lab
For internal lab repos, like fimpy, bouter, etc., you don't have to be overly formal over a bug description; still, having it there is a good way of sharing it with other people who might experience the same (as opposed to a Element chat). Reaction time should not be longer than private messages, and if it is, you can just bother prople via two channels!

### 7.0.1 GitHub issues in external repos
For lab repos developed around the world, make sure you comply to the points described above ([an example](https://github.com/napari/napari/issues/733)). Many packages will provide you with a template for your issue when you start writing on GitHub, so you don't have to worry about how to structure it. But as long as there's all the above, you're fine.

---

## 8 Share your solution!
A very important and underappreciated point: after you have found a solution, let everyone know about it! So post again on the GitHub issue, or go back to the people you have been working on it and let them know what worked in the end. This is crucial to keep improving the collective knowledge, always a good time investment!


