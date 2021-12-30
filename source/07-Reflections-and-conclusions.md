# Conclusions

This thesis explored how to control generative systems with audio signals and create a constant stream of real-time generated artworks. Therefore, the designed system performs well and creates visually pleasing, ever-changing artworks which are regenerated based on features extracted by the incoming audio signal. The artworks are composed of 3D objects, 2D textures, and animations. 

Chapter 2 conveys an overview of generative art and its techniques. It starts with defining what generative art means in the context of this thesis. Afterward, the most common techniques are detailed and explained. Furthermore, the use cases of Generative art are summarized and explained by drawing examples from the Music, Video, and Gaming industry. The chapter finished by explaining the difference between algorithmic and generative art. The used system in this thesis aligns with the definition of Generative Art, not algorithmic art.

The next chapter gives an overview of the basics of Audio Analysis. The term 'Sampling,' the basics of the fast Fourier transformation, and its corresponding window functions are explained. The window function that is used for this bachelor thesis is also outlined. The first part of this chapter describes the techniques to analyze audio files. In contrast, the second part of the chapter goes into more detail about the features that can be extracted from the analyzed audio signal. Some of these features are used for the requirements analysis in chapter 4.

Based on these findings, a requirement analysis has been put together in chapter 4. The requirements are categorized into functional and non-functional requirements. In chapter 5, it is shown that these requirements have been implemented and fulfilled. In 5.1, the input data is discussed, and its general structure is showcased. The characteristics of the music genre are explained and broken down. 5.2.1 promises a performant and easy distributable approach by choosing and native application over a web application. Up to this point, the theoretical groundwork is done, and the project basics are set up. The chapter explains how the Audio is Analyzed, which frequency spectrum is chosen and why. The flow of the incoming signal is broken down and visualized. Afterward, the feature extraction is explained, and the essential features are shown in more detail. These features are converted into Events that are used throughout the application. The second part of the chapter details how the scene is set up and which layers construct a new artwork. The chapter goes into more detail about the used scripts and how they link together with the event system. Some example Assets are discussed to fulfill the requirement of a minimal viable working project. Up to this point, the system aims to implement a proof of concept that shows how the components can work together with each other in order to create artworks. As this thesis is published, the artworks will consist of the example assets shown in 5.2. Unfortunately, there was no time left to present more complex artworks and feature a wide range of different assets. Lastly, the chapter discusses the pros and cons of a monolithic approach and how they compare against the approach taken in this thesis.

## Improvements

The following improvements are discussed. These improvements come with a performance hit that needs to be critically evaluated if this is suitable for the given system.

### Beat detection algorithm

As stated in 3.6, the time between the onsets in the lower frequency range can determine the BPM (Beats per Minute). To detect the BPM, the ms between 3 onsets is measured. By knowing the BPM, a grid can be fitted aligned with the onsets of the low frequency. The Grid allows to count the bars of the musical piece and to predict brakes before the frequency change has even indicated this. One approach could be to change the rerender of the artwork after 128 beats automatically.

### Tonality detection

In 3.5.3, tonality is discussed; tonality is an indicator for the key of the musical piece. Each key conveys a different feeling to the listener. An interesting improvement would be extracting the key and mapping its value to a color. This color could then be used to control the lighting of the scene. The key can also manipulate the outcome of a texture generation algorithm.

### Generated options

The system's generated options make up for a significant improvement to allow for more variance. Having more models and textures to choose from does make the application more interesting and worthwhile. Textures can be generated inside the application by modifying pixel values directly at the program start. A model can be generated too. By stacking and combing random meshes, a new model can emerge. To optimize for performance, these models are created on program start or are generated before the new scene is shown to minimize the perceived performance.

### Transitions

In the base application, the Transition is always the same. With a quick fade to a dark screen, the new scene is rendered in the background and displayed when the fog subsides. This Transition does not change is not generated in any way but rather a static animation. This Transition can be generated. For example, the color fades to change every time a transition is initiated. Furthermore, camera effects are possible. To Transition, the camera could rotate and focus a new point in the space where the new artwork will be rendered.

### Export MIDI

MIDI is the standard for communication between musical instruments and software. Modern lighting and laser systems use midi to communicate the mode they are operated in. This way, the technician can play the lights and lasers just like they would play an instrument. Virtually all modern keyboards, lighting systems, and recording equipment can be set up to communicate with a Computer via MIDI for years. 
In order to make use of that, this application could additionally export MIDI notes via an audio interface. This MIDI data can then be mapped to a musical instrument or a lighting system. The MIDI does contain the extracted audio features mapped to a virtual keyboard. Each Note does represent an event that has been converted from the audio signal. For example, an extended break release could map to the key of C.
