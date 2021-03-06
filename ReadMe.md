# Audio Deep Learning with Tensorflow
----

<figure>
    <center><img src="./Code/Images/Fig_1.jpg" style="width: 100%;" align=”center” /></center> 
    <center><figcaption>Photo by <a href="https://unsplash.com/@pawel_czerwinski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Pawel Czerwinski</a> on <a href="https://unsplash.com/s/photos/sound?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  </figcaption></center> 
</figure>

---

## Problem Statement

voice-controlled assistant tools like Siri, Alexa, Cortona, or Google are becoming more and more part of our daily lives. We use our voice command on these devices to play music, shop online, listen to audio book, turn on/off lights, order food, or check weather. These tools use speech recognition and audio classification to convert a voice command into a format that a computer can understand. 
The objective of this project is to: 
- provide a workflow for audio classification.
- develop a tool that will translate clips of raw audio files, containing a single word, into text. The project will use speech command data as inputs for a Convolutional Neural Network (CNN) to develop a model to classify 30 possible outcomes, using the model's accuracy score (F1).


---

## <a id = 'Content'> Content </b></a>
- [Executive Summary](#ExecutiveSummary)  
- [Library Requirements](#LibraryRequirements)
- [Dataset Description](#DataSetDescription)
- [Audio Sampling](#AudioSampling)
- [Audio Processing](#AudioProcessing)
- [Methodology](#Methodology)
- [CNN](#CNN)
- [Results and Discussion](#Results)    
- [Recommendations](#Recommendations)
- [References](#References)

---

##  <a id = 'ExecutiveSummary'>Executive Summary</b></a>

Part of living in a modern society is to use speech recognition tools to perform daily tasks. Most of us believe these tools help us save time and money at the cost of our data being mined to make these tools more efficient and accurate. However, most of us are not familiar with the technology these tools use behind the scenes. Although the concept of speech recognition is not new to the scientists, its widespread presence in our daily life has started almost a decade ago. New computer processors, cloud computing, and deep leering have made speech recognition and audio classification computationally cheap and accurate enough so it can be assessable to common people. Nowadays, a retail customer can buy Amazon Echo Dot for less than $20. 
Like image processing, convolutional neural networks can be used to translate a raw audio file into a text. Open-source libraries like Librosa, PyTorch, and Scipy in Python can simply read a raw audio file and make it ready for a neural network modeling. One of the challenges in working with audio clips for speech recognition is how fast or slow a speaker talks. Two people can pronounce the word 'Dog' but with different speed such as 'Doog' or 'Dooooggg'. One of the solutions for this challenge is sampling the audio set with a specific rate. The aim of this project is to provides a workflow on audio classification and overcoming its modeling challenge’s.


---

##  <a id = 'LibraryRequirements'>Library Requirements</b></a>

- pandas
- numpy
- matplotlib
- seaborn
- sklearn
- librosa
- tensorflow

---

## <a id = 'DataSetDescription'>Dataset Description</b></a>

Dataset used in this project is from Kaggle, and contains a collection of one-second audio files with a single English word spoken by a variety of different speakers, and separated in 30 folders with the folder name being the label of the audio clip. The audio files were collected using crowdsourcing, check aiyprojects.withgoogle.com/open_speech_recording for more information. Echoing the words of data collectors, the core words are "Yes", "No", "Up", "Down", "Left", "Right", "On", "Off", "Stop", "Go", "Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", and "Nine". To help distinguish unrecognized words, there are also ten auxiliary words, which most speakers only said once. These include "Bed", "Bird", "Cat", "Dog", "Happy", "House", "Marvin", "Sheila", "Tree", and "Wow". According to data collectors, the files contained in the dataset are not uniquely named across labels, but they are unique if you include the label folder. For example, 00f0204f_nohash_0.wav is found in 14 folders, but that file is a different speech command in each folder. The files are named so the first element is the subject id of the person who gave the voice command, and the last element indicated repeated commands. Repeated commands are when the subject repeats the same word multiple times. Subject id is not provided for the test data, and you can assume that most commands in the test data were from subjects not seen in train.

---

##  <a id = 'AudioSampling'>Audio Sampling</b></a>


Sound is a pressure wave produced by a vibrating object. The vibration induces oscillations (parallel to the direction of wave propagation) in the medium particles (usually air molecules) over time. A sound signal is one dimensional and is represented by its characteristics such as amplitude, and period or frequency. Amplitude shows the strength of a signal and frequency indicates the rate of its vibration. Period is the duration of one full wave and frequency (reciprocal of period) is the number of full waves in one second. Most of the sounds that we hear are complex signals and are composed of different signals. We use microphones and speakers to record and play sound signals. A microphone is an analogue-digital converter which translates a sound signal into a series of numbers or an electrical signal. A speaker on the other hand takes an electrical signal and transform it into pressure wave aka sound signal.


<table><tr>
<td bgcolor="ghostwhite"> 
    <figure>
        <center><img src="./Code/Images/Fig_3.gif" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Figure from <a href="https://www.checkinfrared.com/far-infrared-sauna.html">Far Infrared Sauna Explained</a>
      </figcaption></center> 
    </figure>
</td>    
<td bgcolor="ghostwhite">
    <figure>
        <center><img src="./Code/Images/Fig_2.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Figure from <a href="https://www.electroniclinic.com/frequency-modulation-and-amplitude-modulation-fm-and-am-modulation/">Frequency Modulation and Amplitude Modulation, FM and AM modulation</a>
      </figcaption></center> 
    </figure>
</td> </table> 

<p style="margin: 20px 0"></p>

<p style="margin: 20px 0"></p>

To prepare a raw sound signal for audio classification, we need to first digitize it by representing it with an array of numbers. This array of numbers indicate the amplitude of a sound signal at at equally-spaced points over time. This process is called sampling since each measurement is a sample. The audio processing libraries most of the time ask users for sample rate. Sample rate is the number of collected sample in an interval of one second. A sampling rate of 16kHz means that a one-second audio clip is represented by 16000 amplitudes at intervals of 1/16000th second.



<figure>
    <center><img src="./Code/Images/Fig_4.png" style="width: 60%;" align=”center” /></center> 
    <center><figcaption>Figure from <a href="https://commons.wikimedia.org/wiki/File:Signal_Sampling.png">Signal Sampling</a>
  </figcaption></center> 
</figure>

---

## <a id = 'AudioProcessing'>Audio Processing</b></a>


The advancement of Deep Learning in recent years has made development of models for speech recognition easier and more assessable without the need to be a subject matter expert in phonetics. Audio classification with Deep Learning transforms a raw audio clip into a Spectrogram image and uses a convolutional neural network to classify the sound signal. <br>
Sound waves are composed of mixed frequencies. A sound spectrum represents the different frequencies present in a sound. A sound spectrum is like a cooking recipe. In a cooking recipe, the cook needs to mix specific amounts of different ingredients to create a meal. In a sound spectrum, a complicated sound wave is decomposed by [Short-time Fourier Transform (STFT)](https://en.wikipedia.org/wiki/Short-time_Fourier_transform) to its main frequencies and their amplitude. The lowest frequency in a sound signal is called the fundamental frequency. Other frequencies that are an integer multiplier of fundamental frequency are called harmonics. A spectrogram is a visual way of representing the sound loudness of a signal over time at various frequencies present in a particular waveform. The X-axis and Y-axis in a spectrogram represent time and frequency. The color bar indicates the strength of each frequency. The brighter is the color the higher is the energy of the signal. Every vertical line in a spectrogram is the spectrum of the signal at that specific moment and shows the loudness of each constituent frequency at that moment. Neural network is more successful in recognizing patterns in a spectrogram than a sound wave [(Why Mel Spectrograms perform better)](https://towardsdatascience.com/audio-deep-learning-made-simple-part-2-why-mel-spectrograms-perform-better-aad889a93505). <br>
Librosa, scipy, and PyTorch are the available libraries for audio processing in Python. It can be said that between these packages Librosa is one of the most popular, with an extensive set of features. Although torchaudio in PyTorch has fewer functionalities than Librosa, it has been developed specifically for deep learning. These libraries convert a loaded raw audio file into a numpy array.


<figure>
    <center><img src="./Code/Images/Fig_5.jpeg" style="width: 100%;" align=”center” /></center> 
    <center><figcaption>Figure from <a href="https://newt.phys.unsw.edu.au/jw/brassacoustics.html">A crescendo played on a trombone.</a> The spectrogram shows time on the X-axis, and frequency on the Y-axis. The colorbar represents the sound level, on a decibel scale, where blue is weak and red is strong.
  </figcaption></center> 
</figure>


The hearing range if humans include a range from about 20 Hz to 20 kHz. This means that we are more sensitive to lower frequencies than the higher ones. We also perceive sounds in a logarithmic scale rather than a linear scale. The Mel Scale is designed to quantify this intrinsic feature of human life. The mel scale (after the word melody) is a perceptual scale of pitches judged by listeners to be equal in distance from one another [(Wikipedia)](https://en.wikipedia.org/wiki/Mel_scale). The mel scale helps to visualize what typical people can hear and not hear regarding the difference between two sounds. The reference point between the mel scale and normal frequency measurement is defined by assigning a perceptual pitch of 1000 mels to a 1000 Hz tone, 40 dB above the listener's threshold. <br>
The level of a sound loudness, represented by Decibel scale, in our ears helps us to perceive the amplitude of a sound signal. Interestingly, similar to frequency, we hear loudness logarithmically rather than linearly. On the decibel scale, the smallest audible sound (near total silence) is 0 dB. A sound 10 times more powerful is 10 dB. A sound 100 times more powerful than near total silence is 20 dB. A sound 1,000 times more powerful than near total silence is 30 dB. The sound of the jet engine is about 1,000,000,000,000 times more powerful than the smallest audible sound [(What is a decibel, and how is it measured?)](https://science.howstuffworks.com/question124.htm). In speech recognition, we use the mel spectrogram rather than regular spectrogram. The mel spectrogram uses: 
- the mel scale instead of Frequency on the y-axis
- the decibel scale instead of amplitude for the color bar



<table><tr>
<td bgcolor="ghostwhite"> 
    <figure>
        <center><img src="./Code/Images/Fig_6.jpeg" style="width: 200%;" align=”center” /></center> 
        <center><figcaption>Figure from <a href="http://www.cochlea.org/en/hear/human-auditory-range">HUMAN AUDITORY RANGE</a> FREQUENCIES PERCEIVED BY MAN AND SOME COMMON MAMMALS
      </figcaption></center> 
    </figure>
</td>    
<td bgcolor="ghostwhite">
    <figure>
        <center><img src="./Code/Images/Fig_7.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Figure from <a href="http://www.cochlea.org/en/hear/human-auditory-range">HUMAN AUDITORY RANGE </a>Decibel levels of common sounds
      </figcaption></center> 
    </figure>
   
</td> </table> 


---

## <a id = 'Methodology'>Methodology</b></a>

- Import raw audio files as wave files
- Generate a spectrogram for each audio file
- Apply data augmentation like time or frequency mask on spectrogram image
- Split dataset into train and test sets
- Train a convolutional neural network to classify spectrograms in the train set into one of the 30 categories
- Generate prediction for the test set

<p style="margin: 20px 0"></p>

<figure>
    <center><img src="./Code/Images/Fig_8.png" style="width: 90%;" align=”center” /></center> 
    <center><figcaption>Figure from <a href="https://towardsdatascience.com/audio-deep-learning-made-simple-sound-classification-step-by-step-cebc936bbe5"> Sound Classification, Step-by-Step.</a> Workflow for audio classification
  </figcaption></center> 
</figure>

---

## <a id = 'CNN'>Convolutional Neural Network (CNN)</b></a>


Unlike regular neural network, convolutional neural network has extra layers of pre-processing to apply into an input image through Convolution operators, Maxpooling, and Padding. The central idea of the convolution layers is to extract the important features from the image and simplify them (or downscale). The convolution layer consists of a set of filters that take the original image and convolve them into the kernel. The following figure shows the impact of applying 3x3 kernel filter (left), 2x2 Max Pooling (middle), and 3x3 Max Pooling (right) on a 2D vectorized image.

<p style="margin: 10px 0"></p>

<figure>
    <center><img width="1145" alt="image" src="https://user-images.githubusercontent.com/22139918/141344082-d075edfe-4160-4806-9c66-fc2d298e00d1.png"></center> 
    <center><figcaption>Figure from <a href="https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53)">A Comprehensive Guide to Convolutional Neural Networks.</a> 
  </figcaption></center> 
</figure>

In the following, the general script for the build CNNs is represented which includes 2 hidden layers (with 384 and 128 neurons). In addition, one 2D convolutional layer with 6 filters (3x3 kernel) is included. 
<p style="margin: 10px 0"></p>

<figure>
    <center><img src="./Code/Images/Fig_10.png" style="width: 100%;" align=”center” /></center> 
    <center><figcaption> Neural network architecture
  </figcaption></center> 
</figure>

---

##  <a id = 'Results'>Results and Discussion</b></a>


Data augmentation can be applied not only on the raw data (translate an audio file to a mel spectrogram, but also on the mel spectrogram and MFCC images. Frequency and Time masks are two of the frequent forms of data augmentation on mel spectrogram and MFCC images. Frequency mask randomly blocks out a range of consecutive frequencies by adding horizontal bars on the spectrogram. Time mask is like frequency masks, except that we randomly block out ranges of time from the spectrogram by using vertical bars. These techniques help to reduce overfitting in a model, as shown in the result images. The figures below show the performance of masked and unmasked mel spectrograms for the same neural network structure and with the same number of epochs. As it is shown, applying the time and frequency masks reduce the overfitting but it also slightly drops the value of F-1 score.



<table><tr>
<td bgcolor="ghostwhite"> 
    <figure>
        <center><img src="./Code/Images/Fig_11.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Original (unmasked) mel spectrogram image
      </figcaption></center> 
    </figure>
</td>    
<td bgcolor="ghostwhite">
    <figure>
        <center><img src="./Code/Images/Fig_12.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Masked mel spectrogram image
      </figcaption></center> 
    </figure>
</td> </table> 


<table><tr>
<td bgcolor="ghostwhite"> 
    <figure>
        <center><img src="./Code/Images/Fig_13.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Training and testing loss by epoch for unmasked mel spectrogram
      </figcaption></center> 
    </figure>
</td>    
<td bgcolor="ghostwhite">
    <figure>
        <center><img src="./Code/Images/Fig_15.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Training and testing loss by epoch for masked mel spectrogram
      </figcaption></center> 
    </figure>
</td> </table> 


<table><tr>
<td bgcolor="ghostwhite"> 
    <figure>
        <center><img src="./Code/Images/Fig_14.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Classification report for unmasked mel spectrogram
      </figcaption></center> 
    </figure>
</td>    
<td bgcolor="ghostwhite">
    <figure>
        <center><img src="./Code/Images/Fig_16.png" style="width: 100%;" align=”center” /></center> 
        <center><figcaption>Classification report for masked mel spectrogram
      </figcaption></center> 
    </figure>
</td> </table> 

---

##  <a id = 'Recommendations'>Recommendations</b></a>

To reduce overfitting in the trained model, it is beneficial to mix in realistic background audio with the audio clips to help the model to cope with noisy environments. It also helps to train the model longer to improve the score, change the number of MFCC coefficients, change the architecture of CNN, or train the model with RNN.

---

##  <a id = 'References'>References</b></a>

[THE BUSINESS BENEFITS OF VOICE ASSISTANTS](https://zesium.com/the-business-benefits-of-voice-assistants/) <br>
[Frequency Modulation and Amplitude Modulation, FM and AM modulation](https://www.electroniclinic.com/frequency-modulation-and-amplitude-modulation-fm-and-am-modulation/) <br>
[What is a Sound Spectrum?](https://newt.phys.unsw.edu.au/jw/sound.spectrum.html) <br>
[What is a decibel, and how is it measured?](https://science.howstuffworks.com/question124.htm) <br>
[Audio Deep Learning Made Simple (Part 1): State-of-the-Art Techniques](https://towardsdatascience.com/audio-deep-learning-made-simple-part-1-state-of-the-art-techniques-da1d3dff2504) <br>
[Audio Deep Learning Made Simple (Part 2): Why Mel Spectrograms perform better](https://towardsdatascience.com/audio-deep-learning-made-simple-part-2-why-mel-spectrograms-perform-better-aad889a93505) <br>
[Audio Deep Learning Made Simple (Part 3): Data Preparation and Augmentation](https://towardsdatascience.com/audio-deep-learning-made-simple-part-3-data-preparation-and-augmentation-24c6e1f6b52) <br>
[Audio Deep Learning Made Simple: Sound Classification, Step-by-Step](https://towardsdatascience.com/audio-deep-learning-made-simple-sound-classification-step-by-step-cebc936bbe5)<br>
[Machine Learning is Fun Part 6: How to do Speech Recognition with Deep Learning](https://medium.com/@ageitgey/machine-learning-is-fun-part-6-how-to-do-speech-recognition-with-deep-learning-28293c162f7a)<br>
[Data Augmentation for Speech Recognition](https://towardsdatascience.com/data-augmentation-for-speech-recognition-e7c607482e78)<br>
[TensorFlow Speech Recognition Challenge](https://www.kaggle.com/c/tensorflow-speech-recognition-challenge/data)<br>
[Warden P. Speech Commands: A public dataset for single-word speech recognition, 2017](http://download.tensorflow.org/data/speech_commands_v0.01.tar.gz)<br>
[A Comprehensive Guide to Convolutional Neural Networks](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53)



