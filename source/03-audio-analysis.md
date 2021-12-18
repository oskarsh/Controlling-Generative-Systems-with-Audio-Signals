# Audio Anaylsis

## Sampling

Sampling is the reduction of a continous-time signal to a discrete-time signal. In the Audio world this translates to converting a sound wave to a sequence of samples (a discrete-time signal). One sample represents a point in time which holds the value that was converted from the sound wave. To capture a sound wave digitally that enough information is present to fully reconstruct that sound digitally the sample rate needs to be chosen properly. The sample rate defines the resolution that is used to represent a sound wave. Higher values will give more accurate representation of the original sound wave but will require more storage space and bandwidth. The sample rate of a sound wave is not the same as the frequency that represents the pitch of that sound wave. The sample frequency (in Hz) is calculated as the sample rate divided by the length of the sample. The sample rate is actually the frequency of the digital representation of the sound wave, not the frequency of the sound wave itself. So the sample frequency is much higher than the frequency of the sound wave. The Nyquist Theorem states that a sample frequency of at least twice the highest frequency in the sound wave is needed to recover the original sound wave.

The Nyquist Theorem states that in order to fully reconstruct a sound wave from a sampled version of that sound wave, the sample rate has to be at least twice the highest frequency in the sound wave. If the sample rate is less than twice the highest frequency in the sound wave, the sound wave can not be fully reconstructed. 

Here is a example of a poorly choosen sample rate and its reconstruction.

![Showcase of different sample rates and its effects](resources/sample-rate.gif){ width=500px }


## Fast Fourier Transformation

The Fast Fourier Transformation (FFT) has been used for the past decades to obtain the frequency domain representation of the waveforms. In this work, we use the FFT to extract Audio Features of Audio Signals. The FFT is performed on the windowed version of the signal. In this work we make use of the Hamming Function to reduce the effect of the windowing function. The Hamming function is the generator of the second kind of the Z-transform, the Z-transform being widely used in the field of Digital Signal Processing to provide the frequency domain representation of signals. The Hamming function acts as a windowing function, but unlike the windowing functions used in the FFT, it reduces the effect of the windowing function on the extracted features. In this paper, we perform FFT on the windowed version of the signal and then we apply the Hamming function and finally we extract the features. The features are extracted by applying a vector quantization on the frequency domain representation obtained. The results show that the features extracted using the Hamming function are very different from the features extracted using the FFT without the Hamming function. The proposed method is tested on speech signals, music signals and audio signals. The results show that the proposed method is able to distinguish between the different categories of signals.


## Window function

A fft window function works by dividing the signal into a number of segments and then using the same function on each segment. The result is that the frequency components of the signal are preserved and only the magnitude is scaled.

The window function determines the number of segments. This can be done by calculating the length of a side of a triangle. You then divide the length into the number of segments you want.

The window function also determines the number of points that are used in the windowed fft. This means that the windowed fft is only good for a given size of signal.

A triangle window is a good choice for most signals. It is a good trade-off between not losing too much information and not wasting too many operations.
A Hann window is used for signals with a large range of frequencies such as audio signals.
A Hamming window is used for signals that have large amounts of noise, such as images.

fft_sinc

The fft_sinc function does a sinc windowed fft. This function uses half as many points as the triangle window, which makes it twice as fast.

This function is a good choice for signals with a large range of frequencies.

This function is not good for signals with a lot of noise.

This is a good choice for audio signals because it retains the low frequencies and it is fast.

fft_hamming

The fft_hamming function does a Hamming windowed fft. This function is even faster than the fft_sinc function because it uses less points.

This function is a good choice for signals with a lot of noise.
This is not a good choice for audio signals because it removes the low frequencies.

fft_blackman

The fft_blackman function does a Blackman windowed fft. This function is ideal for audio signals because it does not remove the low frequencies.

This function is not good for signals with a lot of noise.
This is not a good choice for signals with a very large range of frequencies.

fft_kaiser

The fft_kaiser function does a Kaiser windowed fft. This function is good for signals with a large range of frequencies and that also have a lot of noise.
This function is not good for audio signals.

This function is not good for signals with a large amount of noise.

fft_blackman_nuttall

The fft_blackman_nuttall function does a Blackman Nuttall windowed fft. This function is very similar to the Blackman windowed fft.

This function is not good for audio signals.
This function is not good for signals with a large amount of noise.


## Extracting Audio Features

Digital analysis of audio signals emerged with the advent of the digital age. Julius O. Smith III can be considered one of the pioneers, who did significant work in the field of physical signal analysis and processing. Today, digital audio analysis is used in DAWs (Digital Audio Workstation), DJ programs and AI voice assistants. The main distinction is between real-time and look-ahead analysis, usually a mix of both systems is used.

There is a range of audio features which can be extracted from a given audio signal through either parametric or non-parametric methods. In parametric methods a frequency table is constructed from the audio signal and the audio features are extracted from the table. In non-parametric methods the audio signal is transformed in a way that makes the feature extractable. For example, in frequency domain, the low frequency components are extracted.

The audio signal can be segmented into frames, usually of size 20ms. The segmented signal can be transformed into different domains to extract the audio features. The domains used are:

Frequency domain: This domain is used to extract the frequency and its harmonics. Usually the signal is first transformed into the frequency domain using a Fourier Transform. This is an efficient method and it is fast. However, it requires a large amount of memory and the computation of the frequency spectrum is not trivial.

Spectral domain: This domain is used to extract the spectral amplitude at each frequency. It is a subset of the frequency domain.

Audio intensity: It is used to extract the volume of the sound.

Spectral correlation: It is used to extract the correlation between the frequencies in the audio signal.

Energy: It is the total energy of the signal. It is computed as the sum of the square of the amplitudes of the frequency components.

Onset Detection: Onset refers to a sudden spike in amplitude. This can be used to detect beat drums in a given musical piece or the beginning of a new phrase. 

Tempo Deteciton: The time between the onsets in the lower frequency range can be used to determine the BPM (Beats per Minute). To detect the BPM the ms between 3 onsets is measured. The formula to calculate the BPM is:

( 60 / MS ) * 1000

\pagebreak





