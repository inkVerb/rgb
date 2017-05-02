# RGBvid

Create R-G-B videos as seen on the inkVerb YouTube channel!
https://www.youtube.com/watch?v=HX46ILgwTNk

## What is this?

This folder/repo contains scripts and libraries to composite and fill the folders with number-ordered .png files for conversion to video.

Look inside every script to see what is going on. The most-detailed explanation script is `copyrgbtomovcount`.

## Install any dependencies for this project

`sudo apt install octave ffmpeg imagemagick git kdenlive`

## Clone the Repo

`git clone https://github.com/inkVerb/rgb`

## Make sure you are working inside the folder

`cd rgb`

## Play with the bubble wrap

`chmod +x install`

`./install`

## Create the solid color images using Octave

1. Open Octave from your desktop menu.

2. Run the scripts in `Octave Scripts`.
Run everything betwen hash comments.

Dependency folder for the Octave SCripts:
- `colors/` (This must be your working directory in Octave!)

## Create the R-G-B panel

This uses the `bin` libraries of individual R, G, and B to create panels to composite on top of solid colors

Run:
`./makergball`

Dependencies, folder and files:

- `colors/` must be populated from the Octave scripts.
- `bin/` must be downloaded and unzipped.
- `makergb` is used by `makergball`
- `rgb-bin/` contains raw R - G - B .png transparencies that need to be composited with the panel frame.
- `rgb-base/` contains the R-G-B transparencies from `rgb-bin/` composited with the panel frame, but have no solid colors.
- `rgb-panel/` contains the R-G-B panel composites from `rgb-base/` AND has the solid colors on the left.

*Note: You can do much more with layers. Most of this is done using* [ImageMagickⓇ][1]*, namely the* `composite` *command, as seen in* `makergb`*. You can use* `makergb` *by following the instructions in the file to make almost any of the 16 million + RGB combinations possible. This only creates rgb-panels that are used in this video.*

## R-G-B video

Copy and order the `rbg-panel/` files to be created by `ffmpeg`

1. Run:

`./copyrgbtomovall`

...but the files have only been populated in `rgb-panel-mov/`. They need to be ordered by numbers that `ffmpeg` can recognize.

2. Run:

`./copyrgbtomovcount`

...Now the files in have been renamed by a number order that `ffmpeg` can recognize.

Use "Create the videos" below for 

## Real Colors for video

1. Composite the color wheel on top of the solid colors

Run:

`./realwheelcolors`

Dependency folders for `realwheelcolors`:

- `alpha-wheel/` (downloaded)
- `colors/` (created from Octave scripts for Real Colors)

*Note: This is a bit tricky because there are only 360 hue levels displayed on the wheel, but there are 1122 levels of color to apply to them. This script matches them correctly.*

2. Composite the wheel-color images with the R-G-B panel

Run:

`./realwheelpanel`

Dependency folders for `realwheelpanel`:

- `alpha-wheel/` (downloaded)
- `rgb-panels/` (created from makergball, see dependencies)

## Create the videos wtih ffmpeg

PNG files are ready for `ffmpeg` to create videos with them in these folders:

- `rgb-panel-mov/`
- `colors-real-wheel-mov/`
- `colors-real-wheel-panel-mov/`

Enter these with:

`cd rgb-panel-mov`

...etc.

Once the -mov directories are created and populated, use this from the terminal inside that folder to create the video:

`ffmpeg -y -framerate 30 -i %5d.png  -c:v libx264 -s:v 1920:1080 moviename.mp4`

*Note: "%5d" means "any 5-digit number" such as "00001". We created all movie-ready .png files with 5 digits so that this simgle ffmpeg command would work with all of them. However, you can use any digit length as long as they match. ffmpeg will ignore any fies with different digit length than specified here.*

After it finishes, it's easy to just move the file to the parent directory at `rgbvid/`

`mv *.mp4 ../`

Then go up one directory to see your video:

`cd ..`

Open it by double clicking on it in your file brower.

*ffmpeg can also be finnicky when converting images into video, or with anything. If you see "drop" in the messages then it's skipping .png files because you have the wrong settings. Here are a few other examples that should work, as well as the source of help after hours of trying to solve this problem myself:*

- `ffmpeg -y -framerate 30 -i Rplot%d.png  -c:v libx264 30f.mp4`
- `ffmpeg -y -framerate 30 -r 30 -i Rplot%d.png -c:v libx264 30fr.mp4`
- `ffmpeg -y -r 30 -i Rplot%d.png -c:v libx264 30r.mp4`

Thanks [Yi Hui!][2] [https://github.com/yihui/animation/issues/74]

## Notes:

1. The video from `colors-real-wheel-mov/` is not part of the original video at the Ink Is A Verb YouTube Channel.
2. The video from `colors-real-wheel-panel-mov/` must be inserted into the video from `rgb-panel-mov/`. Do this at about 10 seconds before the end of the `rgb-panel-mov/` video, when the numbers briefly pause at DD22DD. Use an editor such as Kdenlive for this. And, you must create a video file with 1080 pixels and 30 fps because that is what these videos are.
3. I also made additional edits to the final video on YouTube. I simply inserted some images from the video directories (above) in Kdenlive to make the panel seem to pause.

## Credits & Reference:

- ImageMagickⓇ (`composite` command)
[1]: https://www.imagemagick.org/script/index.php

- Yi Hui (ffmpeg examples)
[2]: https://github.com/yihui/animation/issues/74

