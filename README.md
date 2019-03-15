To run an experiment you run the jar file and give it a few commands : 

```
java -jar encranion-present-assembly-0.1.jar <path to output> <path to experiment definition file> <COM port on the laptop that the ESU is connected to>
```

Let's say you have an experiment defintion file "expdeftest.txt"

To run an experiment you first create an output folder. This is where
the responses and timing of events (relative to the computer's time) 
will be written.

Let's say that this folder is located at "path/to/output/" and the ESU is connected to COM4

Then, to run the experiment, you run

```
java -jar encranion-present-assembly-0.1.jar path/to/output/ expdeftest.txt COM4
```

This will create files

path/to/output/responses.txt
path/to/output/timing.txt

responses.txt has a line for each response. The first item is the "unix epoch time" (search that on google if you need to)
and the second is the key code of the response. You can also google "key codes" if you need to. It should be easy to find a table.

timing.txt has a line for each event. The first item is the "unix epoch time" and the second item is the event.

You can use these two files along with the events in the EDF/bin file to match up the responses. timing.txt also allows you to
see if the ESU has any delay, which is possible. It is my understanding that the timing.txt file should be more accurate with respect
to when (relative to each other) the stimuli were displayed.


### Parsing bin files

To parse the binary files created by this program (it won't necessarily work for past experiments) you run

```
java -jar encranion-present-assembly-0.1.jar readevents /path/to/XXXX_third_party.bin /path/to/where/you/want/the/parsed/file/saved.txt
```


## Experiment specification file

To run an experiment you need to create a file which contains one stimuli per line. 
Currently sounds (.wav) and images (.png, .jpg, .gif, .bmp) are supported.

Markers must be valid signed 4 byte integers (you shouldn't have to worry about that). If you do not want
to have a marker for a given event then the marker value should be `-1`.

The first line of the experiment file must specify the background color of the presentation screen. 
It is specified in RGB format. Each value of R, G, and B must be integers 0-255

```
BACKGROUND R G B
```

The rest of the lines are started specifying the type of stimuli `BLANK` `SOUND` `IMAGE` or `RESPONSE`. 
Each type has it's own parameters:

`BLANK` specifies that the screen should be blank. The the first parameter is the marker and the 
second parameter is the duration in seconds.
The following specifies a 2.5 second blank with marker 54. 

```
BLANK 54 2.5
```

`IMAGE` specifies that an image should be displayed. The image needs to be sized correctly
beforehand. The image will be centered on the screen. The parameters are the marker, the duration
and the FULL path to the image. For example the following shows `/path/to/yourimage.jpg`
for 4.25 seconds with the marker 6

```
IMAGE 6 4.25 /path/to/yourimage.jpg
```

```SOUND``` specifies that a sound should be displayed. The file needs to be a .wav file. The
parameters are the marker and the FULL path to the sound file.
For example the following plays `/path/to/yoursound.wav` and marks the beginning with 14

```
SOUND 14 /path/to/yoursound.wav
```

```RESPONSE``` specifies that the user is supposed to respond with some keypress. The 
key press is logged. The parameters are the marker, the maximum duration, and optionally
an image that is presented (e.g. instructions). The below example as a max response
of 5 seconds and shows image `/image/path/image.jpg` marker with 64

```
RESPONSE 4 exampleimage.png 1 37 exampleimage2.png 39 exampleimage2.png
```

