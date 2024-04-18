

#  feeding mark analysis 

This repository contains scripts for data collection and image processing for quantitative analysis of insect feeding marks on plant leaves, related to publication [**add-link-when-published**].   


## Requirement   

* A photo scanner and its driver software.  

* [UWSC](https://www.vector.co.jp/soft/winnt/util/se115105.html) or alternative keyborad/mouse macro tools. This will be used to operate printer driver software to repeat scan over time by sending key command with a set interval.  

* [ImageJ](https://imagej.net/ij/) or [FiJi](https://fiji.sc/) for image processing. 

* [R]() for data processing and visualization.    


## Workflow  

### Step 1. Image scanning  

For time-lapse scanning, a scanner is operated through its driver software. Instead of manual operation, a key command to start scanning is sent with desired interval by keyboard/mouse macro tool.  
Here is an example of scanning automation with EPSON GTX-980 scanner with UWSC mouse/keyboard macro. 

1. Open scanner driver software.  
2. Set up scanning and saving options. Serial numbers in output filename suffix is essential for the donwstream analysis (ex. `feeding_track_001.jpg`, `feeding_track_002.jpg`, ...).  
3. If needed, open the script `scripts/time_lapse_scan.uws` with text editor and edit the followings:
`interval = 60`  to change the scanning interval (unit: second)  
`KBD(VK_S)` to change the key command to send to the scanner driver. `VK_S` means "S" key, which is a shortcut key of the Epson scanner driver software to start scanning.   
4. Launch UWSC and load the script `scripts/time_lapse_scan.uws`. 
5. Start running the  macro from UWSC. Click the main window of the scanner driver software to make it active. Note that the key command from UWSC will be send to the active window. 


### Step 2. Image processing    

Run ImageJ/Fiji macro, that load the scanned images (.jpg) and process them to quantify leaf area of each ROI and frames.  

1. The ImageJ macro assumes the following file structure. Here is an example of a project (ex. "project1". The project folder name is arbitrary). Put your scanned images with sequential suffix numbers in `project1/raw/`.  

<pre>
project1
├─output 
├─raw
│  ├─feeding_track_001.jpg
│  ├─feeding_track_002.jpg
│  ├─...
├─roi
│  └─RoiSet.zip
└─scripts
</pre>

2. Open one of the scanned image and define ROIs for leaf area quantification  using ROI manager. Save the ROI file (`RoiSet.zip`) in `project1/roi/`.   

3. Open either of macro file in `scripts/` with Fiji/ImageJ.

* `feeding-mark-analyzer-light-bg.ijm` for white background images (i.e. scanner in transmissive mode)    
* `feeding-mark-analyzer-dark-bg.ijm`  for black background images (i.e. scanner in reflective mode)     

 Close any images/results in ImageJ to prevent possible trouble when running the macro. Click "Run" on the macro editor. You will see three dialog boxes for selecting an input file directory(`project1/raw`), a ROI file(`project1/roi/RoiSet.zip`), and an output directory(`project1/output`).  Wait until "Completed !" message appears in the log window. It creates three output directories: 

1. `mask1`  
    Binary JPEG images after color thresholding.  
2. `mask2`  
    Binary JPEG images for leaf detection after cumulative time-difference masking.   
3. `measurement`     
    Excel files for leaf area of each ROI. A file will be created for each image.   

<pre>
project1
├─output 
│  ├─mask1
│  │  ├─feeding_track_001.jpg
│  │  ├─feeding_track_002.jpg
│  │  ├─...
│  ├─mask2  
│  │  ├─feeding_track_001.jpg
│  │  ├─feeding_track_002.jpg
│  │  ├─...
│  ├─measurement
│  │  ├─feeding_track_001.xls
│  │  ├─feeding_track_002.xls
│  │  ├─...
├─raw
│  ├─feeding_track_001.jpg
│  ├─feeding_track_002.jpg
│  ├─...
├─roi
│  └─RoiSet.zip
└─scripts
</pre>


### Step 3. Data processing/visualization   

Calculate/visualize feeding behavior parameters from the image analysis results using R package `feedingMarkAnalyzeR`. 

See [vignette](docs/feedingMarkAnalyzeR.html) in the package for usage.   





