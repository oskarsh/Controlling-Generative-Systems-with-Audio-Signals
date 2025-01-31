# Introduction

The description of the physical processes of sounds or music and their origins has always been an important and widespread research topic. Already the Greeks, especially Euclid and Archytas, tried to describe the properties of music physically and mathematically, for example, the question of the origin of sound and the mathematical description of harmony [^1]. The description of the physical laws also serves the understanding of the studied instruments and can help analyze them more precisely. Nowadays, audio signals are mainly analyzed digitally. This analysis is universally applicable and finds appeal in all areas. Sleep trackers use the sounds at night to determine the sleep phase, AI voice assistants that respond to mood, or systems that can detect malfunctions in mechanical components. There are a variety of character styles that can be extracted from an audio signal. These characteristics are primarily abstract data describing the audio signal. Generative art or process art is a form of contemporary avant-garde art developed in the 1960s based on minimal art and performance art ideas. Some people claimed the first generated art piece was called "Generative Computergrafik" by Ness in February 1965 [CITE] .

This art form is mostly digitally generated and expressed in abstract images, videos, or animations. Early on, the industry adopted these systems and let generative do the work of architects, game designers, and engineers. For the creation of a generative work, it needs certain input data, which are mostly generated randomly under framework conditions. This thesis deals with generating this input data from an audio signal. Randomness also plays an important role here. It is randomly selected which generative System is used. Once this System is initialized, it uses the audio signal as input data. 

## Motivation

Generative System is becoming more and more popular, and their techniques are used for live performances, art exhibitions, and commercial purposes. Artists used many creative approaches and algorithms to create Images, Music, and Videos in the past. The modern Day approaches lean more towards 3D renderings and physical machines. The demands for computers have also changed; complex 3D renderings need heavy computational power and optimizations. This is especially true for live performances and real-time generation. Artists strive for new tools to communicate their vision and find new ways of developing their style. Long-standing techniques like the Mandelbrot [^2] do not qualify for this anymore. So the Mandelbulb [^3] is the next iteration of this famous algorithm, featuring complex 3D scenes and endless artworks with a high grade of Detail. Artists see Generative Art as a tool in their toolbox next to the brush and the Canvas. These systems can generate an endless amount of artworks within a given style. These artworks are created mainly by using controlled randomness.

## Goals

For this Bachelor thesis, a system is created that generates data-driven 3D real-time artworks. The artworks will be generated by layering 3D Models, Textures, and Particle Effects. These artworks will be created semi-randomly, an incoming audio signal is analyzed, and based on the audio features, they will be affected. This includes Camera position, Animation speeds, and general Asset properties. These systems will make them suitable for live audio / visual performances with a low setup cost. Therefore the user interaction will be kept to a minimum, and this will mostly be a plug-and-play system.

For the artworks, there shall be some degree of variance to them. However, since the artworks are created by a system using models and premade textures, there is a certain degree on how much it will differ each time. The goal here is not to create algorithmic art that creates art purely out of code but rather to control a generative system with audio signals that comes with its assets. To use this System in a live audio/video performance, the artist needs to use its models and textures to create a unique style since this application base only comes with generic assets. In addition, the System shall be designed to be easily extensible using Unity3D [^4]. The artist with modeling and some programming skills shall create and load their models to create a unique composition. Additionally, the System shall be built with performance in mind. Finally, the System will be loosely coupled to make it easily extensible and allow other artwork data sources.

## Structure

In the following chapters, basics like Generative Art techniques, Audio Analysis, and related work will be covered. The last chapters of this thesis will focus on the requirements, the design, and implementation and will finish with a retrospect. In retrospect, alternatives and possible future work are discussed.

[^1]:https://www.emis.de/journals/NNJ/Pont-v6n1.html
[^2]:See 2.6.1.2
[^4]:See 6.1.1
[^3]:See 2.6.1.3

