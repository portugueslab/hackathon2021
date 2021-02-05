# Hackathon2021
Folder with the material of the Portugues lab hackaton 2021

## Organisation
The hackathon will consist of alternating sessions of:

1) **Tutorials** in Python and data analysis, with different degrees of difficulty (maybe 1/2 per day, ~0.5-1h long + long intro the first day); people should feel free to assist to the the ones they feel useful for them. The tutorials will happen through notebooks whenever possible, and should have parts with little exercises and not just frontal lecturing.
2) **Work on small projects** in groups of 2 people for the rest of the time. For this people should work at their own pace, ideally in pairs of different coding skills levels. I (Luigi) will be around most of the time to help out people discussing projects and helping figuring things out, so that no one is stuck for too long. At the end of the week, maybe the last afternoon, there can be presentations about what has been done during the week.

## Program
The hackathon will take place from 15th to 19th of February. This is a temptative schedule, that we can revisit as we go:
 - Day 1: (maybe, if enough people want it) long intro to Python ecosystem: Python, Anaconda, conda, Jupyter, PyCharm, git/GitHub (morning); beginning of project work (afternoon)
 - Day 2: Project work + 1/2 lectures (tba)
 - Day 3: Project work + 1/2 lectures (tba)
 - Day 4: Project work + 1/2 lectures (tba)
 - Day 5: Project work (morning); projects presentation (afternoon)
 
 ## Lectures (in progess)
I think we have roughly space for 6-7 plus the intro, depending on their length. here some proposals, to be discussed:
 - Introductions as needed on Python, Anaconda, conda, Jupyter, PyCharm, git/GitHub;
 - Classes and objects in Python (aka, let's get confused together - Luigi?)
 - Introduction to how to use and look at an imaging dataset from the lab with `split-dataset` and `napari` (Hagar)
 - Make sense of your imaging responses with traces filtering methods and clustering (Ot)
 - Speed up your functions vectorising code and using numba - maybe, if people would find it useful (Luigi?)
 - Python packages, pip installation and automatic testing (Luigi)
 
 ## Projects (in progress)
 - Clean up the repo for lightsheet data alignment to reference.  
 Topics: alignment, `ants`, notebooks, `napari`; 
 Priority: high; 
 - Abstract class for stytra hardware, window for arduino control, solve the trigger bug (require access to an arduino/a setup)
 Topics: stytra; multiprocessing; arduino; microscope communication, `zmq`
 Priority: medium  
 - Napari plugin for split datasets  
 Topics: `split_dataset`, `napari`, python package structure, pip deployment
 Priority: high
 - Preprocess a dataset just using `fimpy` and clean up all the little problems and holes in documentation that could be found  
 Topics: fimpy; write documentation; 
 Priority: medium
 - Hack the projector and synchronize it for 2P and lightsheet experiments
 Topics: hardware/electronics project; 
 Priority: high, but lots of "if"s on the feasilibilty (requires hackable projector)
 - Work on Sashimi, as currently single plane acquisition is not supported; (require at least part-time access to the lightsheet setup)
 Topics: `sashimi`, `stytra`, multiprocessing, `zmq`
 Priority: medium

 
