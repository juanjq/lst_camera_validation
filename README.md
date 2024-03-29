# LST-camera validation


Scripts to do the tests for the validation of the LST cameras. 

(Note): About the `lstchain` or `ctapipe` environements, needed in some stages of the analysis, see [https://github.com/cta-observatory](https://github.com/cta-observatory/cta-lstchain). You will need to execute `conda activate lst-dev` (or similar) before opening jupyter notebook.

## Table Of Contents

- [Rate scans analysis](#rate-scans-analysis)

- [Dark pedestal & pedestal with background](#dark-pedestal-and-pedestal-with-background)

- [Pedestal recovery](#pedestal-recovery)

- [Time resolution](#time-resolution)

- [CrossTalk](#crosstalk)

- [Deadtime](#deadtime)

- [Camera plots](#camera-plots)
  * [For plotting the waveforms for the pixels of determined runs](#for-plotting-the-waveforms-for-the-pixels-of-determined-runs)

  * [For plotting an event on the camera](#for-plotting-an-event-on-the-camera)

# Rate scans analysis 
### Instructions to use:

(Note): Recommended to have installed `PyPDF2` package, it can be done with `conda install -c conda-forge pypdf2`, and also not strictly requiered but recommended to use `lstchain` or `ctapipe` environements. For this reason maybe this script is recommended to run in your computer, not at pic.

1. Copy `.result` files from CaCo to your computer or PIC, inside some folder

2. Copy the contents of the github folder `rate_scans` all in same directory, from where we are going to run the script.

3. We need more information than what's inside the files, that we do not have in CaCo, this needs to be written by hand in a file called `extra_data.txt` organised like this, matching the date in the doc and the date of the `.result` file (example of file used in LST-2 in https://github.com/juanjq/LST_camera_validation/blob/main/rate_scans/extra_data.txt)
a
```
date            HV          DAC     neighbor      gain
y-m-d-h:min:s , HV/no_HV , 1/2/3 , 0/1/2/3/4/5 , 0/7/10/15/20
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; And this ones will be the runs that we are going to extract the data. Put this file in same directory of the scripts.

4. Once we have this, we open the notebook **`main.ipynb`**, and complete the requiered parameters,
    - `data_type`, we use `='l0'` if we want to analyse the L0 or ipr runs (pixel analysis), and `='l1'` for the L1 data (cluster analysis)
    - `data_dir`, the full directory name where we have the rate scans data (without final '/'), for example, `'/.../.../results'`

5. Run all the notebook. Plots will be generated in a folder called `output` in same directory where you have the scripts

### Output:

You will get inside the `output` folder generated, a `.pdf` for each run, where you can find all the clusters/pixels fitted, and the different types of analysis of the 50% threshold with different plots, 

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/ratescans.png" align="center" alt="drawing" width="650"/>
</p>

Also a `.pdf` separated with all the analysed data together for all the runs, tabulated in one plot

# Dark pedestal and pedestal with background
### Instructions to use:
(Note): Needed the `lstchain` environement in order to read R0 data.

1. Copy the contents of github folder `dark-background_pedestal` in the same directory

2. First notebook to run is **`create_files_pedestal.ipynb`**, where we need to change the parameters,
    - `RUNS` the array of runs indexes we want to create the data
    - `root` the complete path to all the folders with de data captured with the camera (with final `/`)

3. Run all the notebook. (Pedestal analysis is slow, ~1h per subrun)

4. Once all the files are created we can run all the notebook **`analysis_allRuns.ipynb`**, with the parameters,
    - `RUNS` the array of runs indexes we want to analyse
    - `root` the complete path to all the folders with de data captured with the camera

5. For the analysis of long runs, (the average over minutes, and the 300 last and first seconds), you need to run all the notebook **`analysis_longRuns.ipynb`**, with the parameters,
    - `RUNS` array with the Long Runs
    - `root` the complete path to all the folders with de data captured with the camera (with final `/`)

### Output:

You will get inside the `graphs` folder different outputs:

* The fourier signal (of pedestal, and stdv), and also for the nanosecond scale, for randomly selected pixels, the Histogram of the high frequencies, with the main one specified, histograms of all the data, and camera plots

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/allRuns.png" align="center" alt="drawing" width="1000"/>
</p>

* Plots of the 60 seconds average for long runs (random pixels) and plots of the 300 last and first second analysis, with calculated ratios

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/longRuns.png" align="center" alt="drawing" width="900"/>
</p>

# Pedestal recovery
### Instructions to use:
(Note): Needed the `lstchain` environement in order to read R0 data.

1. Copy the contents of github folder `pedestal_recovery` in the same directory

2. First notebook to run is **`create_files_pedestal.ipynb`**, where we need to change the parameters,
    - `RUNS` the array of runs indexes we want to create the data
    - `root` the complete path to all the folders with de data captured with the camera (with final `/`)

3. Run all the notebook. (Pedestal analysis is slow, ~1h per subrun)

4. Once all the files are created we can run all the notebook **`analysis_pedestalRecovery.ipynb`**, with the parameters,
    - `RUNS` the array of runs indexes we want to analyse
    - `root` the complete path to all the folders with de data captured with the camera
    - `freq` the rate in Hz of the flasher 

### Output:
In the `graphics` folder you will get, the pedestal and stdv in function of the dt, for all and individual pixels, and also a representation of mean values for all the pixels,

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/pedestalRecovery.png" align="center" alt="drawing" width="900"/>
</p>

# Time resolution
### Instructions to use:
(Note): Needed the `lstchain` environement in order to read R0 data.

1. Copy the contents of github folder `pedestal_recovery` in the same directory

2. First notebook to run is **`create_files_timeresolution.ipynb`**, where we need to change the parameters,
    - `RUNS` the array of runs indexes we want to create the data
    - `root` the complete path to all the folders with de data captured with the camera (with final `/`)

3. Run all the notebook.

4. Once all the files are created we can run all the notebook **`analysis_timeResolution.ipynb`**, with the parameters,
    - `RUNS` the array of runs indexes we want to analyse
    - `root` the complete path to all the folders with de data captured with the camera

### Output:
In the `graphics` folder you will get, the arrival time of the peak inside one event, 

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/timeResolution.png" align="center" alt="drawing" width="870"/>
</p>

# CrossTalk
### Instructions to use:
(Note): Needed the `lstchain` environement in order to read R0 data.

1. First of all, as we implemented an optimization process to the file creation (restricting the pixels we look at), data need to be taken in the same way for all the LST validations.
    * Data taken cluster by cluster in runs of 10 clusters, (70 pixels). So in total 27 runs
    * Starting from cluster 1 in CaCo geometry, as we see in the figure,
<p align="center">
  <img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/cluster_crosstalk.png" align="center" alt="drawing" width="350"/>
</p>

2. Once we have taken ALL the runs we copy the contents of github folder `crosstalk` in the same directory

3. First, run all the notebook **`create_files_crosstalk.ipynb`**, changing the parameters,
    - `RUNS` the array of runs indexes we want to create the data
    - `root` the complete path to all the folders with de data captured with the camera (with final `/`)


4. Once all the files are created we can run all the notebook **`analysis_crosstalk.ipynb`**, with the parameters,
    - `runBackground` number of run, where we have background noise with HV in all the camera. In order to have some data to compare, and obtain with that, the crosstalk
    - `root` the complete path to all the folders with de data captured with the camera

### Output:
Inside the created `graphs` folder we find:

* An example of the neighbors selection for some pixels

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/CrossTalk_neighbors.png" align="center" alt="drawing" width="400"/>
</p>

* The histogram of the **%CrossTalk** defined as the formula below, for the 3 different neighbor cases

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/CrossTalk.png" align="center" alt="drawing" width="550"/>
</p>

# Deadtime
Here I only did the script to extract the times data of a run, i.e. the time where the events happen, not any more information. That is the only information we need to perform the Deadtime analysis
(Note): Needed the `lstchain` environement in order to read R0 data.
### Instructions to use:
1. Copy the contents of github folder `deadtime` in the same directory

2. Run the notebook **`create_files_timeonly.ipynb`**, where we need to change the parameters,
    - `RUNS` the array of runs indexes we want to create the data
    - `RATES` the rates used in each run, only used for the name of the file
    - `root` the complete path to all the folders with de data captured with the camera (with final `/`)

### Output:
CSV files for each run with the timestamp of all events.


# Camera plots
(Note): Needed the `lstchain` environement
## For plotting the waveforms for the pixels of determined runs
### Instructions:

1. Copy the notebook **`waveforms_plots.ipynb`** from this github folder: `plot_on_camera`

2. Change the parameters inside the notebook:

   * `RUN` Run number
   * `Nevents` Events to extract (a small number recommended < 100)
   * `root` Root folder with the location of the runs; runs inside a folder with the name of the respective date.

3. Go to the respective block for only 1 event plot or multiple plots, and run the notebook

### Output:
he plot of the waveforms for all the pixels,

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/waveforms.png" align="center" alt="drawing" width="500"/>
</p>

## For plotting an event on the camera
### Instructions:

1. Copy the notebook **`camera_plots.ipynb`** from this github folder: `plot_on_camera`

2. Change the parameters inside the notebook:

   * `RUN` Run number
   * `Nevents` Events to extract (a small number recommended < 100)
   * `root` Root folder with the location of the runs; runs inside a folder with the name of the respective date.

3. Go to the respective block for only 1 event plot or multiple plots, and run the notebook

### Output:
The plot of the mean waveform values for respective event plotted on the camera

<p align="center">
<img src="https://github.com/juanjq/LST_camera_validation/blob/main/graphs/camera_plot.png" align="center" alt="drawing" width="500"/>
</p>

---

