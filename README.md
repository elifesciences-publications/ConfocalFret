### This code is associated with the paper from Rashid et al., "Single-molecule FRET unveils induced-fit mechanism for substrate selectivity in flap endonuclease 1". eLife, 2017. http://dx.doi.org/10.7554/eLife.21884

# FRETmaker
FRETmakerv14f is a matlab script designed to help process, organzie, and visualize FRET trajectories of single molecules. This script is specifically analyzes with trajectories exproted as **separate** donor and acceptor time traces from the SymPhoTime *FRET* script. The script outputs a histogram of the FRET states from all traces. Therefore, in a given session, all traces shoudl be from the same condition/experiment.
This script interacts with the user primarily through text, but nearly all steps and syntax are explained by the script to the user to make for relatively intuitive use.
## Output files
FRETmakerv14f can make several output files, upon the users instruction which have various uses. First is a m.mat file which stores all relevent data in a form that the FRETmakerv14f script can read, which allows for a given set of files already processed to be re-examined at a later date. For processing with [HAMMY](http://ha.med.jhmi.edu/resources/) , which needs a different file format than exported by SymPhotime, FRETmakerv14f can re-export each trace, removing the bleached end, or splitting traces with blinking into multiple traces, into a HAMMY readable format, while also generating a special file which allows the dwellHAMMYf2 script to read coalate and analyze the results of processing with HAMMY.
## Input format
Traces are loaded as two .dat files, when loading traces you will be prompted to enter in the file names of donor and acceptor traces, enter the filenames of each time trace as *donorfilename.dat* *acceptorfilename.dat* hitting enter after each pair. After the last pair is entered, hit enter and type Y and enter to indicate all traces of teh given condition are entered, and begin the next phase. 
Traces acquired in SymPhoTime should be exported by processing with the FRET script, selecting appropriate bin size and then right clicking on the donor trace window, selecting export this window, and saving a .dat file, do the same for the acceptor window. The script is insensitive to the name scheme, however, it is *strongly* recomended that you 
1. Begin the file name with *bin size*msbin-
2. Name the donor and acceptor files identically except for an identifier signally whether it is a donor or acceptor file
The format commonly used is this: 
**bin size**msbin-**SymPhoTime file name**-D.dat for the *donor trace*
and
**bin size**msbin-**SymPhoTime file name**-A.dat for the *acceptor trace*
For example: 
5msbin-200nmFEN1WT-BTdfDNA-duplexlabeled3_x56.02µm_y21.41µm_z40.00µm_1-D.dat is a Donor trace
5msbin-200nmFEN1WT-BTdfDNA-duplexlabeled3_x56.02µm_y21.41µm_z40.00µm_1-A.dat is an Acceptor trace
Begninning with the bin size is more important in this project, as the dwellHAMMYf2 script uses this **bin size*msbin- to identify the bin size and allow it to process the results of HAMMY files, as HAMMY removes the bin size information from its result files. Using a system of identical naming except for donnor and acceptor identifier is just common sense so that file pairs are easily identified, reducing the chance of a mixup. The script does contain some checking functions verifying that you are using donor traces as donor traces- SymPhoTime includes a header for this in the .dat file, and that the traces are the same length, but still without a system, mistakes are very easy to make. It is also prefered to use in the middle, the name of the file in the SymPhoTime workspace that the trace was created from as this allows easy reference from trace seen in the script back to the original source data file.

#dwellHAMMYf2
dwellHAMMYf2 is used for visualizing and further processing the results of HAMMY processing of a group of smFRET traces from the same experimental condition, such as analyzing dwell time and comparing the distribution of states, and creating a transition density heat map.
##Usage
dwellHAMMYf2 is designed to operate with a combination of text and GUI interfaces, with most important information explained to the user with text prompts in the command line.
The primary piece of information needed is the name of the list file generated. This will load all HAMMY processed files *make sure to process all files exported from FRETmakerv14f with HAMMY before running this script* and then prompt user to cycle through all traces, visualizing the HAMMY fit along with the raw trace, with the option to keep or exclude each trace, if the HAMMY fit is not good. Click close at any time to move on to the next phase, but note that traces are included by default, so clicking close before cycling through all traces -note the x/total progress ticker at the top- could mean bad fits are still included, though that may be desired. After this enter the minimum trace lenght,measured in ms, followed by what ranges you want to group FRET states and treat all FRET states from HAMMY as a single FRET state. Note that you can enter as many non-overlapping ranges as you like, but once done, hit enter without having entered any number, to move on to finish the script.
##Output
dwellHAMMYf2 outputs to the console 4 variables, the first is a 2xN cell array, with cells in the first row identifying the FRET range covered by the list in the second row of the cell array of the same column. The list is a 1xX double of all dwell times in ms HAMMY found that have a FRET within the given FRET range. The second output is a structure containing the traces, HAMMY fits and other critical parameters, which is useful in reviewing the data. The last two outputs are both single number statisics, the first being the total number of transitions analyzed in the traces selected to be included in the analysis, and the second begin the total number of traces included in the analysis.
