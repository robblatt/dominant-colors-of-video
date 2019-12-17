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

frame_path = "images/vid_frame/"
try:
    os.makedirs(frame_path)
except OSError:
    print ("Creation of the directory %s failed" % frame_path)
else:
    print ("Successfully created the directory %s " % frame_path)


dominant_colors = {}
average_colors = {}

filelist = glob.iglob(path + '*.png')

j = 0

for filepath in sorted(filelist):
    img = io.imread(filepath) #[:, :, :-1]

    pixels = np.float32(img.reshape(-1, 3))

    n_colors = 5
    criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 200, .1)
    flags = cv2.KMEANS_RANDOM_CENTERS

    _, labels, palette = cv2.kmeans(pixels, n_colors, None, criteria, 10, flags)
    _, counts = np.unique(labels, return_counts=True)

    dominant = palette[np.argmax(counts)]
    
    myimg = cv2.imread(filepath)
    avg_color_per_row = np.average(myimg, axis=0)
    avg_color = np.average(avg_color_per_row, axis=0)
    
    dominant_colors[filepath[8:].split('.')[0]] = dominant
    average_colors[filepath[8:].split('.')[0]] = avg_color
    
    indices = np.argsort(counts)[::-1]   
    freqs = np.cumsum(np.hstack([[0], counts[indices]/counts.sum()]))
    rows = np.int_(img.shape[0]*freqs)

    dom_patch = np.zeros(shape=img.shape, dtype=np.uint8)
    for i in range(len(rows) - 1):
        dom_patch[rows[i]:rows[i + 1], :, :] += np.uint8(palette[indices[i]])
        
    fig, (ax0) = plt.subplots(1, 1, figsize=(5,5))
    ax0.imshow(dom_patch)
    ax0.set_title('Dominant colors')
    ax0.axis('off')
    ax0.text(0,660,'Frame ' + filepath)
    
    if j < 10:
        fig.savefig(frame_path + 'dom_colors_pyplot_00' + str(j) + '.png')
    elif j < 100:
        fig.savefig(frame_path + 'dom_colors_pyplot_0' + str(j) + '.png')
    else:
        fig.savefig(frame_path + 'dom_colors_pyplot_' + str(j) + '.png')
    j += 1
    
    plt.close()


filenames = sorted(glob.glob(frame_path + '*.png'))

images = []
for filename in filenames:
    images.append(imageio.imread(filename))
imageio.mimsave('color_palatte_2.gif', images)

df_d = pd.DataFrame.from_dict(dominant_colors).transpose()
df_a = pd.DataFrame.from_dict(average_colors).transpose()

df_d.reset_index(inplace = True, drop = True)
df_a.reset_index(inplace = True, drop = True)

df_a = df_a.round(0).astype(int)
df_d = df_d.round(0).astype(int)

remove_path = 'images/'
try:
    shutil.rmtree(remove_path)
except OSError:
    print ("Deletion of the directory %s failed" % remove_path)
else:
    print ("Successfully deleted the directory %s" % remove_path)
    
im = Image.new('RGB', (len(df_d) * 10,100)) # create the Image of size 1 pixel 

for j in range(100):
    for i in range(len(df_d)):
        for k in range(1, 10):
            colors = (df_d[0][(i)], df_d[1][i], df_d[2][i])
            im.putpixel(((i * 10) + k,j), colors) # or whatever color you wish

im.save('dominant_pixel_2.png') # or any image format

im = Image.new('RGB', (len(df_a) * 10,100)) # create the Image of size 1 pixel 

for j in range(100):
    for i in range(len(df_a)):
        for k in range(1, 10):
            colors = (df_a[0][(i)], df_a[1][i], df_a[2][i])
            im.putpixel(((i * 10) + k,j), colors)

im.save('average_pixel_2.png') # or any image format
