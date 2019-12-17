# Detect Dominant Colors in Frames of Video

This is a pretty dirty first pass at having an understanding of the dominant colors of a video. It was an idea that came to me during a job interview and wanted to see if I could get it working quickly. It's admittedly rough, especially around temporarily creating a directory, writing files to it, reading those files and then removing that directory.

The output is:
- a gif of a five color palate for each frame of the video
- a png of the dominant color for each frame
- a png of the average color for each frame

The input is the following video from the Museum of Ice Cream's Instagram account: https://www.instagram.com/p/B3IHhXZgz2Y/

Here's the palatte gif for that video, which shows the top five colors used and their representation in each frame:

![Palatte gif](https://raw.githubusercontent.com/robblatt/dominant-colors-of-video/master/color_palatte_2.gif)

The PNG showing the average color for each frame. The colors averaged together makes it pretty useless because the colors shown aren't actualy represented in the videos (they are averages of RGB values), but I still wanted to show it to prove it:

![Average color per frame](https://raw.githubusercontent.com/robblatt/dominant-colors-of-video/master/average_pixel_2.png)

The PNG showing the dominant color for each frame. This makes more sense visually.

![Average color per frame](https://raw.githubusercontent.com/robblatt/dominant-colors-of-video/master/dominant_pixel_2.png)

```
import cv2
import numpy as np
from skimage import io
import glob
import pandas as pd
from PIL import Image
import imageio
import os
import shutil

path = "images/vid_cap/"
try:
    os.makedirs(path)
except OSError:
    print ("Creation of the directory %s failed" % path)
else:
    print ("Successfully created the directory %s " % path)
    
    
cap= cv2.VideoCapture('sample_video_2.mp4')
i=0
while(cap.isOpened()):
    ret, frame = cap.read()
    if ret == False:
        break
    if i < 10:
        cv2.imwrite(path + '00' + str(i) + '.png',frame)
    elif i < 100:
        cv2.imwrite(path + '0' + str(i) + '.png',frame)
    else:
        cv2.imwrite(path + str(i) + '.png',frame)
    i+=1

cap.release()
cv2.destroyAllWindows()
