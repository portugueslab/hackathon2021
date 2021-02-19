# Final results of the lab hackathon!

## Group name
- overall results of the project;
- where the code contributed to during the project is (repo, PRs, PyPI uploads, etc);
- what have you learned that other people might find useful; also links for nice tutorials, etc
- problems you have run into/things you still don't understand

## Sashimi
- Done: Pull request for Planar Imaging mode and no microscope mode done, Automatically updating documentation for sashimi;
- to be found in sashimi repo on github;
- Learned: print statements are your friend :), dont push the meerge button on github, flakeing and blackening for pretty code
- Still to do: bug fixes in saving for planar mode

## Morphing
- I now have a basic idea of how to align datasets with ANTsPy
- Code added so far can be found in the `pipeline_notebooks` subfolder inside the `functional-to-reference` lab repository. Very preliminary so far.
- Basic usage of the wonderful BrainGlobe Atlas API, installation and setting up of the Windows Subsystem for Linux, basic workflow for alignment with the ANTsPy library.
- ToDo's: Finish documenting the computer setup process, properly comment all notebooks, make a guide/REAMDE with a guide for the whole morphing process and suggestions, find a way to store all files and transforms in a slighlty non-confusing way.

## Napari plugin
- Set Github Actions, published a split-dataset/hdf5 reader plugin
- In napari-split-dataset repo and on PyPI
- Linting, pytest, Github Actions, deployment
- Dealing with compatibility. The core of the problems seems to be h5py, which is used in split-dataset.
