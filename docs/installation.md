### Install PySAR

For Mac users, Xcode with command line tools are needed. Skip this part if you have already done so.

1. Install XCode from App store
2. Install command line tools from within XCode and agree to the terms of license.

```tcsh   
xcode-select --install -s /Applications/Xcode.app/Contents/Developer/ 
sudo xcodebuild -license 
```   

3. Install [XQuartz](https://www.xquartz.org), then restart the terminal.

If you want to use Macports to install your Python environment, continue; otherwise (use pip, anaconda, etc.), skip the following and go to the next section: 1. Prepare source file.

4. Install [macports](https://www.macports.org/install.php) and update the port tree with the following command:
```tcsh
sudo port selfupdate
```

Try [Portfile Sync via a Snapshot Tarball](https://trac.macports.org/wiki/howto/PortTreeTarball), if your network prevent the use of rsync or svn via http of port tree.

5. Restart terminal

#### 1. Prepare source file    

To use the package, you need to setup the environment. Depending on your shell, you may use commands below to setup pysar, by adding the following to your source file. They are for:   
1. To make pysar importable in python, by adding the path to PySAR directory to your _$PYTHONPATH_    
2. To make utility scripts available in command line, by adding _${PYSAR_HOME}/pysar_ and _${PYSAR_HOME}/sh_ to your _$path_.   
   
For csh/tcsh user, add to your **_~/.cshrc_** file for example:   

    ############################  Python  ###############################
    if ( ! $?PYTHONPATH ) then
        setenv PYTHONPATH ""
    endif
    
    ##--------- Anaconda ---------------## 
    setenv PYTHON3DIR    ~/python/anaconda3
    setenv PATH          ${PATH}:${PYTHON3DIR}/bin
    
    ##--------- PySAR ------------------## 
    setenv PYSAR_HOME    ~/python/PySAR       #for released version, "~/python/PySAR-0.4.0"
    setenv PYTHONPATH    ${PYTHONPATH}:${PYSAR_HOME}
    setenv PATH          ${PATH}:${PYSAR_HOME}/pysar:${PYSAR_HOME}/sh
   
For bash user, add to your **_~/.bashrc_** file for example:   

    ############################  Python  ###############################
    if [ -z ${PYTHONPATH+x} ]; then export PYTHONPATH=""; fi
    
    ##--------- Anaconda ---------------## 
    export PYTHON3DIR=~/python/anaconda3
    export PATH=${PATH}:${PYTHON3DIR}/bin
    
    ##--------- PySAR ------------------## 
    export PYSAR_HOME=~/python/PySAR        #for released version, "~/python/PySAR-0.4.0"
    export PYTHONPATH=${PYTHONPATH}:${PYSAR_HOME}   
    export PATH=${PATH}:${PYSAR_HOME}/pysar:${PYSAR_HOME}/sh   

Source the file for the first time. It will be sourced automatically next time when you login.
   
   
#### 2. Install Python dependecies
PySAR relies on the following Python modules. We recommend using [Anaconda](https://www.anaconda.com/download/) for Linux users and [Macports](https://www.macports.org/install.php) for Mac users to install the python environment and the prerequisite packages.
- [Python3.6](https://www.anaconda.com/download/)
- Numpy
- Scipy
- h5py
- Matplotlib
- multiprocessing
- [scikit-image](http://scikit-image.org)
- Basemap (optional, for plotting in geo coordinate)
- [pyresample](http://pyresample.readthedocs.org) (optional, for geocoding)
- [pykml](https://github.com/yunjunz/pykml) (optional, for Google Earth KMZ file output)
- joblib (optional, for parallel processing)
- lxml (optional, for ISCE XML file parsing)
- [PyAPS](http://earthdef.caltech.edu/projects/pyaps/wiki/Main) (optional, for tropospheric correction using weather re-analysis models, i.e. ERA-Interim, NARR, MERRA)

Run the following in your terminal (using conda):   

    cd ~/python
    wget https://repo.continuum.io/archive/Anaconda3-5.1.0-MacOSX-x86_64.sh
    chmod +x Anaconda3-5.1.0-MacOSX-x86_64.sh
    ./Anaconda3-5.1.0-MacOSX-x86_64.sh -b -p $PYTHON3DIR
    
    $PYTHON3DIR/bin/conda config --add channels conda-forge
    $PYTHON3DIR/bin/conda install basemap joblib lxml pyresample --yes   
    
    git clone https://github.com/yunjunz/pykml.git; cd pykml
    $PYTHON3DIR/bin/python setup.py build     
    $PYTHON3DIR/bin/python setup.py install    
   
Note that pykml through conda supports python2 only, we provide a python2/3 compatible version [here](https://github.com/yunjunz/pykml.git) and installed throught the command line above by default.
  
For PyAPS installation, please refer to [PyAPS's Wiki at Caltech](http://earthdef.caltech.edu/projects/pyaps/wiki/Main)