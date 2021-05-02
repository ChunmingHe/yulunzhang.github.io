# Description

These are the steps to build code that has Python/C++ code interoperability.

1. Login to a ThetaGPU head node
   ```
   ssh thetagpusn1
   ```

2. Request an interactive session on an A100 GPU
   ```
   qsub -n 1 -q default -A datascience -I -t 1:00:00
   ```
   Following this, we need to execute a few commands to get setup with an appropriately optimized TensorFlow. These are:

3. Activate the TensorFlow 2.2 Singularity container:
   ```
   singularity exec -B /lus:/lus --nv /lus/theta-fs0/projects/datascience/thetaGPU/containers/tf2_20.08-py3.sif bash
   ```

4. Setup access to the internet
   ```
   export http_proxy=http://theta-proxy.tmi.alcf.anl.gov:3128
   export https_proxy=https://theta-proxy.tmi.alcf.anl.gov:3128
   ```
   
5. Now that we can access the internet, we need to set up a virtual environment in Python (these commands should only be run the first time)
   ```
   python -m pip install --user virtualenv
   export VENV_LOCATION=/home/rmaulik/THETAGPU_TF_ENV # Add your path here
   python -m virtualenv --system-site-packages $VENV_LOCATION
   source $VENV_LOCATION/bin/activate
   python -m pip install cmake
   python -m pip install matplotlib
   python -m pip install sklearn
   ```
   `cmake` is required to build our C++ app and link to Python, and other packages may be pip installed as needed in your Python code. An example `CMakeLists.txt` file for building with Python/C interoperability with examples can be found [here](https://github.com/argonne-lcf/sdl_ai_workshop/tree/master/04_Simulation_ML/ThetaGPU).
