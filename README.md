# BEHR-LNOx

This repository contains all the utilities needed to run BEHR.

## Clone

The various components of this version of BEHR-LNOx are linked as Git submodules, so they haven't actually been cloned yet.

To do so, Initialize the repository’s submodules by running `git submodule init` followed by `git submodule update`.

```
git submodule init
git submodule update
```

If you want to use https link for clone, run these command in this directory

```
sed -i 's/git@github.com:/https:\/\/github.com\//g' .gitmodules
git submodule init
git submodule update
```

If a future update to this repo updates submodules, then after "git pull" you will need to run "git submodule update" again.

## Config

`./config` will check the environment variable and paths used in BEHR algorithm.

It's better to set the environment variable 'BEHR' by yourself, like adding this line to `.bashrc`

```
export BEHR=****/BEHR-LNOx
```

## Structure of repositories

```
├── BEHR-core
├── BEHR-core-utils
├── BEHR-PSM-Gridding
├── config
├── data
├── BEHR_Files
├── GLOBE_Database
├── MODIS
│   ├── Land_Water_Mask_7Classes_UMD.hdf
│   ├── MCD43D
│   │   ├── yyyy
│   └── MYD06_L2
│   │   ├── yyyy
├── OMI
│   ├── OMNO2
│   │   └── version_3_3_0
│   │   │   ├── yyyy
│   │   │   │   ├── mm
│   └── OMPIXCOR
│       └── version_003
│   │   │   ├── yyyy
│   │   │   │   ├── mm
├── SP_Files
└── WEBSITE
    └── staging
├── dirs
├── LICENSE
├── Matlab-Gen-Utils
├── MatlabPythonInterface
├── README.md
└── WRF_Utils
```

## Download data

**GLOBE database**

https://www.ngdc.noaa.gov/mgg/topo/gltiles.html and https://www.ngdc.noaa.gov/mgg/topo/elev/esri/hdr/

**Land_Water_Mask_7Classes_UMD.hdf**

ftp://rsftp.eeos.umb.edu/data02/Gapfilled/

**OMNO2 and OMPIXCOR | MCD43D07, MCD43D08, MCD43D09, MCD43D31 and MYD06_L2**

check this [BEHR-Automation](https://github.com/zxdawn/BEHR-Automation) which support downloading and sorting automatically

If you prefer downloading data by wget from web, please check this link: [MODIS](https://ladsweb.modaps.eosdis.nasa.gov/search/) and [OMI](https://disc.gsfc.nasa.gov/information/howto/5761bc6a5ad5a18811681bae/how-to-download-data-files-from-https-service-with-wget).

### Check data whether complete

You won't get error while running the script below, if the data is OK.
```
myDir = 'data_directory'; %gets directory
myFiles = dir(fullfile(myDir,'*.hdf')); %gets all hdf files in struct
for k = 1:length(myFiles)
  baseFileName = myFiles(k).name;
  fullFileName = fullfile(myDir, baseFileName);
  fprintf(1, 'Now reading %s\n', fullFileName);
  hdfinfo(fullFileName)
end
```

## Modify settings

1. Initialize date_start, date_end, region and DEBUG_LEVEL in `BEHR-core/Read_Data/read_main.m`;
2. Initialize parameters of `BEHR-core/BEHR_MainBEHR_main.m`;
3. Compile omi according to help.txt;
4. Initialize parameters of `BEHR-core/HDF tools/BEHR_publishing_main.m`

## Run

If you don't have permission to change Matlab path, you should add BEHR to Matlab path every time:

`addpath(genpath('your_BEHR_path'))`

Run `read_main.m` `BEHR_main.m`  and `BEHR_publishing_main.m` in Matlab successively.

If you want to run in parallel, please add these lines before running:

```
 global onCluster;
 onCluster = true;
 global numThreads;
 numThreads = ??; (?? = your numthreads)
```

## Check ouput

Check files in `data/WEBSITE/staging/behr-daily-us_native-hdf_v3-0B` or `data/WEBSITE/staging/behr-daily-us_gridded-hdf_v3-0B`
