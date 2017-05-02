# RGB (the video)

Create R-G-B videos as seen on the inkVerb YouTube channel!
https://www.youtube.com/watch?v=HX46ILgwTNk

## What is this?

The video was made entirely using Ubuntu.

This repo contains Bash scripts, libraries, and Ubuntu terminal commands to composite and fill the folders with number-ordered .png files for conversion to video.

Look inside every script to see what is going on. The script with the most-detailed explanation is `copyrgbtomovcount`.

Here's how to do it...

## I. Install any dependencies for this project

`sudo apt install octave ffmpeg imagemagick git kdenlive`

## II. Clone the Repo

`git clone https://github.com/inkVerb/rgb`

## III. Make sure you are working inside the folder

`cd rgb`

## IV. Play with the bubble wrap

`chmod +x install`

`./install`

## V. Create the solid color images using Octave

1. Open Octave from your desktop menu.

2. Run the scripts in `Octave Scripts`.
Run everything betwen hash comments.

Dependency folder for the Octave SCripts:
- `colors/` (This must be your working directory in Octave!)

## VI. Create the R-G-B panel

This uses the `bin` libraries of individual R, G, and B to create panels to composite on top of solid colors

Run:
`./makergball`

Dependencies, folder and files:

- `colors/` must be populated from the Octave scripts.
- `bin/` must be downloaded and unzipped.
- `makergb` is used by `makergball`
- `rgb-bin/` contains raw R-G-B .png transparencies that need to be composited with the panel frame.
- `rgb-base/` contains the R-G-B transparencies from `rgb-bin/` composited with the panel frame, but have no solid colors.
- `rgb-panel/` contains the R-G-B panel composites from `rgb-base/` AND has the solid colors on the left.

*Note: You can do much more with layers. Most of this is done using* [ImageMagickⓇ][1]*, namely the* `composite` *command, as seen in* `makergb`*. You can use* `makergb` *by following the instructions in the file to make almost any of the 16 million + RGB combinations possible. This only creates rgb-panels that are used in this video.*

## VII. R-G-B video

Copy and order the `rbg-panel/` files to be created by `ffmpeg`

1. Run:

`./copyrgbtomovall`

...but the files have only been populated in `rgb-panel-mov/`. They need to be ordered by numbers that `ffmpeg` can recognize.

2. Run:

`./copyrgbtomovcount`

...Now the files in have been renamed by a number order that `ffmpeg` can recognize.

Use "Create the videos" below for 

## VIII. eal Colors for video

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

## IX. Create the videos wtih ffmpeg

PNG files are ready for `ffmpeg` to create videos with them in these folders:

- `rgb-panel-mov/`
- `colors-real-wheel-mov/`
- `colors-real-wheel-panel-mov/`

Enter these with:

`cd rgb-panel-mov`

...etc.

Once the -mov directories are created and populated, use this from the terminal inside that folder to create the video:

(Of coruse, put whatever name you want for `moviename.mp4`, as long as it is an .mp4 file)

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

## X. Video editor

I used Kdenlive to finish the video. You could probalby use any video editor to get the same or similar results, but you'll have to experiment to find out.

The video is 1920x1080 30 fps with literally every .png image with its own frame in the video. IMHO, more fps would have been heavier and fewer would have been either a longer video or would have skipped frames.

#### Notes on video editing:

1. The video from `colors-real-wheel-mov/` is not part of the original video at the Ink Is A Verb YouTube Channel.
2. The video from `colors-real-wheel-panel-mov/` must be inserted into the video from `rgb-panel-mov/`. Do this at about 10 seconds before the end of the `rgb-panel-mov/` video, when the numbers briefly pause at DD22DD. Use an editor such as Kdenlive for this. And, you must create a video file with 1080 pixels and 30 fps because that is what these videos are.
3. I also inserted some images from the `colors-real-wheel-panel-mov/` directory (via `pause-cheat` in `goodies/`) in Kdenlive to make the color chooser wheel jump to 24 standard colors around the wheel in the "grand finale". They were six frames per image, of course at 30 fps.
4. For music, I used the extended remix from the SoundCloud download for *LYFO – High* [https://soundcloud.com/lyfomusic/high][3]. I made very few changes to the timing of the video to make it fit the music, using Kdenlive.

## Credits & Reference:

- ImageMagickⓇ (`composite` command)
[1]: https://www.imagemagick.org/script/index.php

- Yi Hui (`ffmpeg` command examples)
[2]: https://github.com/yihui/animation/issues/74

- LYFO – High (music)
[3]: https://soundcloud.com/lyfomusic/high

