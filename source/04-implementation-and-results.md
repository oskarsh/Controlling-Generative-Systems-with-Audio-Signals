# Design and implementation

## Components
This application consists of 3 Parts. The Audio Analysis, the event system and the rendering. These parts build a chain where firstly the Audio Signal is analyzed, then translated into events and finally based on these Events the artworks are rendered.

The Artwork consists of 5 Layers, each Layer does feature a generator to individually react on the Events send by the event system.

### Native Application

The main use case of a native application will be to provide performance optimizations and easy access to the GPU and the periphirals. The application can be executed on every x86 platform while providing similar features. Using Unity3D allows for GPU optimizations, this way we can render the artworks on the GPU while keeping the analysis and data translation to the CPU. This approach will diversify the workload and allows for higher performances.

Unity3D does feature its own build process which allows to compile the application for a wide variety of platforms. This application is targeting the desktop operating systems mainly GNU/ Linux, Windows and MacOS and a native version can be compiled for each system. To build a native application for a desktop Unity first compiles the C# code to an intermediate language. This language is used alongside the meta data to make up the byte code. The mono runtime is able to execute that bytecode and interpret it on each platform accordingly.

![Overview of the Unity build process ,IL = Intermediate Language](resources/unity-compile.png){ width=500px }

\pagebreak

### Audio Analysis

A list of all Input devices can be obtained using Unity3D. Usually the Operation System does flag one input as the primary one. The application will start to listen to input data using the primary input. The data is analysed with the fast fourier transform alogrithm using the blackman window function.

The information inside a Audio Spectrum is not equally split up. Instead of following a equal distribution it followes a unequal distribution. This means that there is much more information between 20 - 60hz than there is between 10000 - 10040hz. To analyse the audio it is needed to have certain channels with specific frequency data. The lowest channels would feature the bass while the higer channels feature the treble. Since the audio signal does have 20000hz individual values it is best to dumb down the values, usually into 8 channels.

For further analysis the Input data will be devided into 8 Channels. Based on this pattern:

20 - 60hz
60 - 250hz
500-2000hz
2000-4000hz
4000-6000hz
6000-20000hz

Adding to this there is also a average of all channel computed which will be holding the amplitude. Since we are listening to non normalized input data there is no way to know how high the values are going to be. Dealing with a quite Signal will result in really low values while loud signals do result in high values. To deal with that uncertanity a normalization process is used. To normalize the values the frequency value of each channel is divided by the highest recorded frequency for that specific channel. This will result in normalized values between 0 and 1. To adapt to certain loudness changes the higest frequency will be constantly decreased by a factor of 0.005. This is used to always re evaluate the highest frequency and to quickly adapt to loudness changes.

To smooth out the values for each channel there is a smoothing process when the values are decreased. Instead of  apruptly jumping to a low value it is decreased gradually. This effect can be seen when visualizing each channel. The animation is less jumpy and more smoothed out.

![Audio Analysis flow](resources/analysis-flow.png){ width=500px }

\pagebreak

### Audio Feature extraction

There are 2 kinds of audio features which can be extracted. Computed features give a rough understanding of the Audio signal, these include energy levels, Frequency domain and spectral spread. These features are computed directly from the frequency of the audio signal and work with only one snapshot of the frequency. Furthermore these values are not normalized by default which make them a not reliable option to analyze realtime audio which is prone to change in tonality, amplitude and energy. 


#### Onset detection

The second kind of audio features is deteremined by algorithms and cannot be determined by applying to a frequency snapshot. These algortihms take past input into account and feed them into the next iteration. Each channel is buffered and saved temporarily to be able to compare to the incoming audio. This enables one of the most used features in this project called onset detection. The onset detection algorithm works by comparing the buffer to the current audio signal and evaluates if a sudden change in aplitude occurs. This algorithm can be fine tuned to detect big spikes in audio that likely appear after long breaks or to listen to subtle changes for example the beating of a drum. 


![visualizing onset detection. RED=onset BLUE=normal audio](resources/onset-detection.png){ width=250px }

\pagebreak

#### Break detection

One of the features implemented was the break detection algorithm. The break detection can differentiate between long and short breaks. These detection algortihms are specifically tailored at the choosed genre of music that is used since they use a reference to compare the current running audio signal against. A break is indicated by a low low-end and little energy in the treble. To detect a break the detection algorithm looks at the first channel of the deconstructed audio to compare the average of the buffered values against the reference values. To classify for a long break the average of the first channel is not allowed to reach the threshold within a given timeframe. To save processing power the algorithm looks at the audio signal 3 times each one second apart. Each iteration of the break detection algorithm the previous state of the run is saved. When the average of the first channel is not able to cross the threshold on 3 consecutive runs of the detection algorithm, a long break ready event will be triggered. This long break ready event indicates that the song currently is in a long break phase. With the next low-end onset this phase will end and the saved values that are used for the long break detection algorithm are cleared again. 

The values of the first channel are compared against reference values which are defined in the application. These reference values do contain averages of the first channel of various reference songs. The reference songs have choosen to feature a range of different audio characteristics. Based on tonality, spectral spread and intensity these songs have been analysed and converted into a machine readable format. The average of the first channel of each reference track is computed and concatinated into a list to serve as a threshold reference for the realtime audio. 

![long break detection algorithm visualized](resources/long-break.png){ width=500px }

\pagebreak

To detect a short break the same mechanism is used, the only difference is that instead of using 3 saved values to detect a break only 1 is used. A onset of the bass after one successfull iteration of the break algorithm indicates a short break. Afterwards the saved values will also be cleared. 


![short break detection algorithm visualized](resources/short-break.png){ width=500px }


### Feature conversion

To make use of the analyzed features they need to be converted into a machine readable form. Unity3D offer an event system that lets the application send events and make them globally accessible. These events can contain pieces of data or extra information. Usually a event consists of a name and additionally some data. Using an event system provides an easy way to communicate data in between the components and allows for loose coupling. The event system acts as a middleware between the audio analysis and the graphical user interface by providing an easy api. In contrast to modern web development the event system dispatches events globally and the client subscribes to them instead of calling the event system for updates.

Having an event system does also allow for easy extensibility in form of plugins. The analysed audio could also be translated into midi data, text input or raw pixels. More on this in the [Outlook](## Outlook) section. 

![Coupling of components](resources/components.png){ width=250px }

\pagebreak

### Scene setup

The scene is made up out of 5 Layers, a processing unit, a camera and lights.

The Camera is wrapped with another component which can be controlled individually. 
The processing unit does provide the AudioPeer which acts as the AudioAnalyser and the EventSystem which converts the audio featuers into events and sends them accordingly.
Each inidividual layer does contain a Script which dynamically loads a Model or Visual Effect. These Scripts are listening for Events and modify there parameters accordingly.
Finally there are some global lights to illuminate the scene.

![Unity3D Scene Hierachy](resources/unity-layers.png){ width=250px }

### Generators

Each layer does feature its own Generator Script. This setup makes sure that each layer is rendered in the right position and does feature the right assets and models. In general there are 5 Layers each assigned to its own Generators. The Generator Script in itself is simple and generic. It exposes a list which can be filled in the Unity Editor UI with GameObjects. When initialized one random GameObject from that list is choosen and displayed. While its technically possible to assign every GameObject to a certain Generator it is advised to reserve the upper Layers for smaller Objects which can be easily layered above the rest. The Layers are stacked across the Z axis to minize clipping.

Here is the code for the Generator Script:

```c#
void AddRandomGameObject()
{
	var r = Random.Range(0, GameObjects.Length);
    var tp = transform.position;
    var position = new Vector3(tp.x,tp.y, tp.z)
	GameObject gb = GameObjects[r];
	var randomObj = GameObject.Instantiate(gb, position, Quaternion.identity);
	randomObj.transform.parent = gameObject.transform;
}
```

\pagebreak 

### Tunnel

The so called tunnel is one of the main elements in the scene. It builds the base of the scene by providing an ever changing background. In itself it layers 4 Textures and moves them forward or backwards following a cylindrical shape. It features properties like movement speed, rotation and twist per each layer. One of the most used feature is the dynamic texture assignment via an API. While it is possible to control each layer on its own, the global control does unify most of them. Similar to the properties on each layer there is a global speed and rotation next to a global brightness.

On the application start it slowly gets brighter. This is due to the fact to hide the adjusting phase of the audio analysis. When starting out the maximum frequency could not be determined yet thus the normalized values do not take the whole spectrum into account. This usually shows itself in really shaky values which tend to jump between high and low.


![Screenshot of the background tunnel](resources/tunnel.png){ width=500px }

\pagebreak

### Fractals

The mandalas are used as a secondary visual. These are meant to be layered on top of Layer 1 and act as a backdrop for the rest of the scene. These mandalas are generated with p5.js and included in the final project.

 p5.js is free and open-source javascript library that runs on the web. Generally p5 is used for experimental web projects, data visualizations and generative art.

This is the code to generate the mandalas

```javascript
const makeMandala = (position) => {
  const layers = layerConstructors.map((lcon) => {
    let picker = random(1);
    const draw = picker > lcon.weight;
    return lcon.init({
      position,
      draw,
    });
  });
  return layers;
};
```

This makeMandala function does generate a random number between 0 and 1. This value is then compared against a weight. This weight defines the likely hood that a specific layer is choosen. This is controlled randomness 

Each layer constructor does contain a name, init and weight field. The name does make the debugging process easier by providing a human readable identifier for the drawn shape. The init function does contain the actual computation of the layers. This can be the drawing of a hexagon, a line or a group of squares. The weight indicates the likely hood that this particular shape is choosen as explained above.

Here is the code for a basic shape that either draws a hexagon, a circle or square background.

```javascript
const layerConstructors = [
  {
    name: "Shape",
    init: (props) =>
      outlineShape({
        ...props,
        ...setState(state),
      }),
    weight: 0.3,
  },
];
```



This relatively simple code is able to generate an endless amount of fractals with a given style. This style is defined by the weight of each layer, the init function and the color palette. In itself this is a generative system which is used to generate parts for another generative system. In order to save on performance these visuals are rendered before hand and are saved as an Image. Displaying an Image does come with a much lower performance overhead then creating the image in realtime. 

![rendering of the isolated fractals](resources/mandalas.png){ width=500px }

\pagebreak

Each of these layers is rendered with a transparent background and does contain only black and white colors. This allows to easily re color the fractals in the final application to match the general theme better. 

![layering the fractals with the background](resources/layered-fractal.png){ width=400px }

![regenerating the layers and background with the background](resources/layered-fractal-2.png){ width=400px }

\pagebreak

#### Model Creation 

![Wireframe model of a Head](resources/head-1.png){ width=250px }

#### Secondary Visual




#### Particle System

#### Background Graphics


![Wireframe model of a Head](resources/apgaen-1.png){ width=550px }


### Generative Camera movements

### Transitions

## Input Data

![waveform of a modern trance song](resources/full-waveform.png){ width=500px }

## Conclusion

## Lessons Learned

### Unity3D

### Performance of realtime systems
