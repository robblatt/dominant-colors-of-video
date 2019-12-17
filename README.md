# Detect Dominant Colors in Frames of Video

This is a pretty dirty first pass at having an understanding of the dominant colors of a video. It was an idea that came to me during a job interview and wanted to see if I could get it working quickly. It's admittedly rough, especially around temporarily creating a directory, writing files to it, reading those files and then removing that directory.

The output is:
- a gif of a five color palate for each frame of the video
- a png of the dominant color for each frame
- a png of the average color for each frame

The input is the following video from the Museum of Ice Cream's Instagram account: https://www.instagram.com/p/B3IHhXZgz2Y/

{::options parse_block_html="false" /}

<div align="center">
    <blockquote class="instagram-media" data-instgrm-captioned data-instgrm-permalink="https://www.instagram.com/p/B3IHhXZgz2Y/?utm_source=ig_embed&amp;utm_campaign=loading" data-instgrm-version="12" style=" background:#FFF; border:0; border-radius:3px; box-shadow:0 0 1px 0 rgba(0,0,0,0.5),0 1px 10px 0 rgba(0,0,0,0.15); margin: 1px; max-width:540px; min-width:326px; padding:0; width:99.375%; width:-webkit-calc(100% - 2px); width:calc(100% - 2px);"><div style="padding:16px;"> <a href="https://www.instagram.com/p/B3IHhXZgz2Y/?utm_source=ig_embed&amp;utm_campaign=loading" style=" background:#FFFFFF; line-height:0; padding:0 0; text-align:center; text-decoration:none; width:100%;" target="_blank"> <div style=" display: flex; flex-direction: row; align-items: center;"> <div style="background-color: #F4F4F4; border-radius: 50%; flex-grow: 0; height: 40px; margin-right: 14px; width: 40px;"></div> <div style="display: flex; flex-direction: column; flex-grow: 1; justify-content: center;"> <div style=" background-color: #F4F4F4; border-radius: 4px; flex-grow: 0; height: 14px; margin-bottom: 6px; width: 100px;"></div> <div style=" background-color: #F4F4F4; border-radius: 4px; flex-grow: 0; height: 14px; width: 60px;"></div></div></div><div style="padding: 19% 0;"></div> <div style="display:block; height:50px; margin:0 auto 12px; width:50px;"><svg width="50px" height="50px" viewBox="0 0 60 60" version="1.1" xmlns="https://www.w3.org/2000/svg" xmlns:xlink="https://www.w3.org/1999/xlink"><g stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"><g transform="translate(-511.000000, -20.000000)" fill="#000000"><g><path d="M556.869,30.41 C554.814,30.41 553.148,32.076 553.148,34.131 C553.148,36.186 554.814,37.852 556.869,37.852 C558.924,37.852 560.59,36.186 560.59,34.131 C560.59,32.076 558.924,30.41 556.869,30.41 M541,60.657 C535.114,60.657 530.342,55.887 530.342,50 C530.342,44.114 535.114,39.342 541,39.342 C546.887,39.342 551.658,44.114 551.658,50 C551.658,55.887 546.887,60.657 541,60.657 M541,33.886 C532.1,33.886 524.886,41.1 524.886,50 C524.886,58.899 532.1,66.113 541,66.113 C549.9,66.113 557.115,58.899 557.115,50 C557.115,41.1 549.9,33.886 541,33.886 M565.378,62.101 C565.244,65.022 564.756,66.606 564.346,67.663 C563.803,69.06 563.154,70.057 562.106,71.106 C561.058,72.155 560.06,72.803 558.662,73.347 C557.607,73.757 556.021,74.244 553.102,74.378 C549.944,74.521 548.997,74.552 541,74.552 C533.003,74.552 532.056,74.521 528.898,74.378 C525.979,74.244 524.393,73.757 523.338,73.347 C521.94,72.803 520.942,72.155 519.894,71.106 C518.846,70.057 518.197,69.06 517.654,67.663 C517.244,66.606 516.755,65.022 516.623,62.101 C516.479,58.943 516.448,57.996 516.448,50 C516.448,42.003 516.479,41.056 516.623,37.899 C516.755,34.978 517.244,33.391 517.654,32.338 C518.197,30.938 518.846,29.942 519.894,28.894 C520.942,27.846 521.94,27.196 523.338,26.654 C524.393,26.244 525.979,25.756 528.898,25.623 C532.057,25.479 533.004,25.448 541,25.448 C548.997,25.448 549.943,25.479 553.102,25.623 C556.021,25.756 557.607,26.244 558.662,26.654 C560.06,27.196 561.058,27.846 562.106,28.894 C563.154,29.942 563.803,30.938 564.346,32.338 C564.756,33.391 565.244,34.978 565.378,37.899 C565.522,41.056 565.552,42.003 565.552,50 C565.552,57.996 565.522,58.943 565.378,62.101 M570.82,37.631 C570.674,34.438 570.167,32.258 569.425,30.349 C568.659,28.377 567.633,26.702 565.965,25.035 C564.297,23.368 562.623,22.342 560.652,21.575 C558.743,20.834 556.562,20.326 553.369,20.18 C550.169,20.033 549.148,20 541,20 C532.853,20 531.831,20.033 528.631,20.18 C525.438,20.326 523.257,20.834 521.349,21.575 C519.376,22.342 517.703,23.368 516.035,25.035 C514.368,26.702 513.342,28.377 512.574,30.349 C511.834,32.258 511.326,34.438 511.181,37.631 C511.035,40.831 511,41.851 511,50 C511,58.147 511.035,59.17 511.181,62.369 C511.326,65.562 511.834,67.743 512.574,69.651 C513.342,71.625 514.368,73.296 516.035,74.965 C517.703,76.634 519.376,77.658 521.349,78.425 C523.257,79.167 525.438,79.673 528.631,79.82 C531.831,79.965 532.853,80.001 541,80.001 C549.148,80.001 550.169,79.965 553.369,79.82 C556.562,79.673 558.743,79.167 560.652,78.425 C562.623,77.658 564.297,76.634 565.965,74.965 C567.633,73.296 568.659,71.625 569.425,69.651 C570.167,67.743 570.674,65.562 570.82,62.369 C570.966,59.17 571,58.147 571,50 C571,41.851 570.966,40.831 570.82,37.631"></path></g></g></g></svg></div><div style="padding-top: 8px;"> <div style=" color:#3897f0; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:550; line-height:18px;"> View this post on Instagram</div></div><div style="padding: 12.5% 0;"></div> <div style="display: flex; flex-direction: row; margin-bottom: 14px; align-items: center;"><div> <div style="background-color: #F4F4F4; border-radius: 50%; height: 12.5px; width: 12.5px; transform: translateX(0px) translateY(7px);"></div> <div style="background-color: #F4F4F4; height: 12.5px; transform: rotate(-45deg) translateX(3px) translateY(1px); width: 12.5px; flex-grow: 0; margin-right: 14px; margin-left: 2px;"></div> <div style="background-color: #F4F4F4; border-radius: 50%; height: 12.5px; width: 12.5px; transform: translateX(9px) translateY(-18px);"></div></div><div style="margin-left: 8px;"> <div style=" background-color: #F4F4F4; border-radius: 50%; flex-grow: 0; height: 20px; width: 20px;"></div> <div style=" width: 0; height: 0; border-top: 2px solid transparent; border-left: 6px solid #f4f4f4; border-bottom: 2px solid transparent; transform: translateX(16px) translateY(-4px) rotate(30deg)"></div></div><div style="margin-left: auto;"> <div style=" width: 0px; border-top: 8px solid #F4F4F4; border-right: 8px solid transparent; transform: translateY(16px);"></div> <div style=" background-color: #F4F4F4; flex-grow: 0; height: 12px; width: 16px; transform: translateY(-4px);"></div> <div style=" width: 0; height: 0; border-top: 8px solid #F4F4F4; border-left: 8px solid transparent; transform: translateY(-4px) translateX(8px);"></div></div></div></a> <p style=" margin:8px 0 0 0; padding:0 4px;"> <a href="https://www.instagram.com/p/B3IHhXZgz2Y/?utm_source=ig_embed&amp;utm_campaign=loading" style=" color:#000; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:normal; line-height:17px; text-decoration:none; word-wrap:break-word;" target="_blank">SWEET things are just around the corner...🍦✨Can you guess?👀👇🏽</a></p> <p style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; line-height:17px; margin-bottom:0; margin-top:8px; overflow:hidden; padding:8px 0 7px; text-align:center; text-overflow:ellipsis; white-space:nowrap;">A post shared by <a href="https://www.instagram.com/museumoficecream/?utm_source=ig_embed&amp;utm_campaign=loading" style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:normal; line-height:17px;" target="_blank"> Museum of Ice Cream</a> (@museumoficecream) on <time style=" font-family:Arial,sans-serif; font-size:14px; line-height:17px;" datetime="2019-10-02T19:03:14+00:00">Oct 2, 2019 at 12:03pm PDT</time></p></div></blockquote> <script async src="//www.instagram.com/embed.js"></script>
    </div>

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
