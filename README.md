# PhysIO-scripts
## author of this readme: s-kline, 9.10.2017
These scripts are for creating movement regressors from cardiac data for inclusion in the single subject analysis.

Before running the physio correction, you should look at the .pptx for some theory and/or read the paper (The PhysIO Toolbox for Modeling Physiological Noise in fMRI Data; Kasper et al., 2017) for more insight.

The main function is physio_job_batch, the other .m-files are for different folder structures (LeAD) or other analysis steps (multisession, GLM etc)
You can call physio_job_batch with callbatch.m from matlab or physio_batch.bat to call matlab and spm from the windows cmd.
The arguments you need to specify are described at the top of physio_job_batch
Depending on your scanning parameters you need to adjust some inputs like number of slices, TR etc, check physio_job_batch thoroughly!

result is a .txt file for each subject with 7 regressors:
1 = cos φ of estimated cardiac phase (6 RETROICOR regressors; Glover et al., 2000)
2 = sin φ of estimated cardiac phase 
3 = cos 2φ ""
4 = sin 2φ ""
5 = cos 3φ ""
6 = sin 3φ ""
7 = Heart Rate Variablity (HRV; Chang & Glover, 2009): heart rate convolved with the cardiac response function (CRF)

If you also have respiratory data, this will add another 7 regressors (RETROICOR and respiration convolved with respiratory response function), interaction regressors (cardiacXrespiratory) and the order might be different (see slide 47 in the .pptx)

if you added the movement regressors from the preprocessing, those come after the physio. 6 additional columns.

****the current agreed-on proceeding for quality control:
	Go through the Matlab output and look for warnings and errors that ocurred during processing the subjects.
	PhysIO_qualicheck contains a basic decision tree to handle this (probably needs to be expanded). 


****Things to look out for in general:
	Sometimes warnings for skipped heartbeats, weird timing or other problems are displayed in the matlab command window and/or the figures.
	Always look through the warnings and check the figures (everything aligned properly, does the timing match with how long your scanning was etc.
	
	Depending on how long before or after the actual scan time the physio measurement was running, this batch might throw an error when using 
	the first dicom as a timestamp, the you need to adjust the individual subject batch to use the last dicom.
	
	Sometimes using first or last dicom will get different results (for example some skipped heartbeats or none) presumably because the 'overhanging' end of the physio file can be noisier than the start or vice versa.
	The current procedure is to use the first dicom as a default and try the last if there are any problems. Then choose the period of time that generates the best regressors (aka no warnings or less skipped heartbeats)
	
	If there are less than 20 skipped heartbeats you can turn on post-hoc selection  in the individual subject batch and mark them by hand.

