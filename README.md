# Distinguishing individuals based on their EEG signals using a tribrid ConvNet with reciprocal attention

## Project description

The goal of this project was to train a convolutional neural network to classify EEG signals as belonging to one of two individuals performing a counting task. 

The project was designed as a binary classification task where the model learned to distinguish between 
two given subjects performing the same "colorRound" task (counting squares of a chosen color). 

Training was repeated for 7 randomly selected pairs of subjects, using 2 different data folds per pairing and the architecture described above. 

## Dataset

[UC Berkeley-Biosense Synchronized Brainwave Dataset](https://www.kaggle.com/datasets/berkeley-biosense/synchronized-brainwave-dataset)

The dataset contains 1-second samples of single-channel EEG readings sampled at 512 Hz. The signals were recorded using a consumer-grade headset with a single-electrode
while the subjects responded to different types of stimuli. 

## Data Augmentation

To increase the number of available training samples, a cropped training strategy inspired by Schirrmeister et al. (2017) was used. For each subject, all samples recorded during the 
"colorRound" stimulus were concatenated in chronological order. New 1-second samples were then generated as sliding time windows, such that the start and end of each sample i+1 
was shifted by 375 ms in relation to sample i. 

## Model Architecture

Models with larger kernel sizes in the convolutional layers were found to be more powerful but have extremely high variance. 
It was found that more stable results could be achieved by using a "tribrid" architecure which allowed for a low-level representation 
of the input to flow through three different processing paths. These paths had the same overall architecture - four 2-layer convolutional blocks followed by two blocks of dense layers -
but different kernel sizes in the convolutional layers. 

The outputs of the processing paths were concatenated, and additive (self) attention was applied, before they were merged by summation.  
The idea was for the paths to "collaborate" by focusing on different features in the input, but taking into account their counterparts' representations.

## Contents of the Repository

 - EEG_data_inspection.ipynb

 - 

## Dependencies

pandas 2.2.2
numpy 1.26.4
matplotlib 3.10.0
sklearn 1.6.1
keras 3.8.0
tensorflow 2.18.0

## References

Chuang, J., Merrill, N., Maillart, T., & Students of the UC Berkeley Spring 2015 MIDS Immersion Class. (2015, May 9). 
*Synchronized Brainwave Recordings from a Group Presented with a Common Audio-Visual Stimulus*. UC Berkeley Spring 2015 MIDS Immersion Class.

Mukherjee, S. (2022, November 5). *Electroencephalogram Signal Classification for action identification*. https://keras.io/examples/timeseries/eeg_signal_classification/

Schirrmeister, R. T., Springenberg, J. T, Fiederer, L. D. J., Glasstetter, M., Eggensperger, K., Tangermann, M., Hutter, F., Burgard, W., & Ball, T. (2017). 
Deep Learning With Convolutional Neural Networks for EEG Decoding and Visualization. *Human Brain Mapping, 38*(5), 5391–5420. 
