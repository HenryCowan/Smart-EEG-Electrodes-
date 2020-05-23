# Masters database

This repository holds four data sets of EEG measurements, python scripts to process 
the data, C++ scripts used to obtain the data and the ethical approval form
 obtained from the Univerity of Glasgow.
 
## The data
 
The data sets can be devided into two broad catagories. Commercial and
Novel.  The Commercial data is data obtained from using a commercial
Dormo Ag/AgCl electrode. The novel data was obtained from using a
self-fabricated annular ring shaped electrode (Ag/AgCl). From initial
comparison of the data sets the signal quality from each electrode
appears similar. The eyes open and eyes closed data were recorded for
120 seconds.
  
The data files are saved in tab-seperated value files with 4
columns. The first column contains the sample index number. The second
column contains the data obtained from one differential channel. The
second column contains data from a second differential channel. The
fourth column contains a time stamp of the oddball stimuli (i.e. a one
where the oddball stimuli flashed).
 
For all commercial recordings, only one differential channel was
recorded (channel 1 , column 2 of the .tsv files). For the novel
recordings both differential channels were recorded, one for the inner
ring of the novel electrode and one for the outer ring of the novel
electrode. The purpose of recording two channels for the novel
electrode was to use the signal from the outer ring of the novel
electrode as a noise input to the LMS filter and remove this from the
signal from the inner ring, resulting in a cleaner EEG.

Videos were recorded for the later subjects to visually observe artefacts during the recordings.
There is an audible countdown before the record button was pressed for each participant to let
you know the starting time.
 
## Code
 
The C++ files are taken from Dr. Bernd Porr. These files are used to record signals
from the Attys biosignal amplifier. 
 
Further information on the Attys Biosignal amplifier can be found here:
 
https://www.attys.tech/
 
For directions of use of the attys.ep files check out GlasgowNeuro's
github. The files presented in my repository have the added
functionality (when compared to GlasgowNeuro's original files) of
being able to save the data into the .tsv files as well as adding the
4th column of oddball time stamp data.
 
https://github.com/glasgowneuro/attys-ep
 
 
There are 5 python files presented in this repository. The first, LMS.py, is 
a python script which implements Bernd Porr's LMS filter alogoritm. To use this 
script one must download Porr's Fir1 filter library which can be found here
(https://github.com/berndporr/fir1) with instruction on how to do so along with 
a description and example of the LMS filter workings. 

The second script, PreProcessing.py is a simple outline for a user-specified 
FIR filter. This script was used to remove any bassline drift, DC and mains from
the data provided BEFORE LMS application. 

The Third and fouth scripts, ProcessingNovel.py and
ProcessingCommercial.py are scripts which used to process the subject
data. Python's 'glob' function was used to plot all data sets
simultaneously however this is not nessecary. These scripts shows how
the data were split into the four seperate channels, the application
of the PreProcessing.py fir filter (bandstop at 0-1Hz and 48-52Hz),
the applicationg of the LMS.py LMS filter (learning rate at 50000 and
250 taps - only for ...Novel.py) and then a subplot of the time and
frequency domain plots from the two differential attys channels after
preprocessing and the time and freq domain plots after LMS filtering
was applied (only for ...Novel.py).

The fifth script, RealtimeLMS.py is a script which working in
combination with the AttysComm software written by Bernd Porr. This
script allows for realtime application of the LMS alogorithm when
simultaneously recording the reference noise and noisy signal. It was
found that pre-processing this data to remove baseline drift was
essential, otherwise the LMS algorithm destabilised. This was
attempted using iir filtering however running, two iir filters, 3 rt
plots and the LMS algoritm simultaneously prooved too much for python
resulting in a not-so -realtime output. Further work must be done on
this.
