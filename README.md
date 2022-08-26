# Overview

Deep-STORM is a single molecule localization microscopy code for training a custom fully convolutional neural network estimator and recovering super-resolution images from dense blinking movies:

![](Figures/DemoExpData.gif "This movie shows representative experimental frames of a microtubules experiment from the EPFL challenge website with localizations overliad as red dots (Fig. 6c main text).")

# System requirements

* The software was tested on a *Linux* system with Ubuntu version 16.04, and a *Windows* system with Windows 10 Home.  
* Training and evaluation were run on a standard workstation equipped with 32 GB of memory, an Intel(R) Core(TM) i7 − 8700, 3.20 GHz CPU, and a NVidia GeForce Titan Xp GPU with 12 GB of video memory.
* Prerequisites
    1. ImageJ >= 1.51u with ThunderSTORM plugin >= 1.3 installed.
    2. Matlab >= R2017b with image-processing toolbox.
    3. Python >= 3.5 environment with Tensorflow >= 1.4.0, and Keras >= 1.0.0 installed.

# Installation instructions

* ImageJ and ThunderSTORM
    1. Download and install ImageJ 1.51u - The software is freely available at https://imagej.nih.gov/ij/download.html
    2. Download ThunderSTORM plugin `<*.jar>` file from https://zitmen.github.io/thunderstorm/
    3. Place the downloaded `<*.jar>` file into ImageJ plugins directory
    * To verify ThunderSTORM is setup correctly, open ImageJ and navigate to the Plugins directory. The ThunderSTORM plugin should appear in the list.

* Matlab can be downloaded at https://www.mathworks.com/products/matlab.html

* Python environment
    1. The [conda](https://docs.conda.io/en/latest/) environment for this project is given in `environment.yml`. To replicate the environment on your system use the command: `conda env create -f environment.yml` from within the downloaded directory. This should take a couple of minutes.
    2. After activation of the environment using: `conda activate deep-storm`, you're set to go!

# Usage and demo examples

* To train a network, the user needs to perform the following steps:
    1. Simulate a tiff stack of data frames, with known ground truth positions in an accompanying csv file, using ImageJ ThunderSTORM plugin. The simulated images and positions are saved for handling in Matlab.
    2. Generate the training examples matfile in matlab using the script `GenerateTrainingExamples.m`.
    3. Open up the anaconda command prompt, and activate the previously created `deepstorm` environment by using the command: `activate deepstorm`.
    4. Train a convolutional neural network for the created examples using the command: `python Training.py --filename <path_of_the_generated_training_examples_mfile> --weights_name <path_for_saving_the_trained_weights_hdf5_file> --meanstd_name <path_for_saving_the_normalization_factors_mfile>`

* To use the trained network on data for image reconstruction from a blinking movie, run the command: `python  Testing.py --datafile <path_to_tiff_stack_for_reconstruction> --weights_name <path_to_the_trained_model_weights_as_hdf5_file> --meanstd_name <path_to_the_saved_normalization_factors_as_mfile> --savename <path_for_saving_the_Superresolution_reconstruction_matfile> --upsampling_factor <desired_upsampling_factor> --debug <boolean (0/1) for saving individual predictions>`
    * Note: The inputs `upsampling_factor` and `debug` are optional. By default, the `upsampling_factor` is set to 8, and `debug` is set to 0.
 
* There are 2 different demo examples that demonstrate the use of this code:
    1. `demo1 - Simulated Microtubules` - learning a CNN for localizing simulated microtubules structures obtained from the EPFL 2013 Challenge (Fig. 4 main text). It takes approximately 2 hours to train a model from scratch on a Titan Xp. See the [**pdf instructions**](https://github.com/EliasNehme/Deep-STORM/blob/master/demo1 - Simulated Microtubules/demo1.pdf) inside this folder for a detailed step by step application of the software, with snapshots and intermediate outputs.
    2. `demo2 - Real Microtubules` - pre-trained CNN on simulations for localizing experimental microtubules (Fig. 6 main text).
