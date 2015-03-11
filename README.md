# human-ffmpeg
A project for making ffmpeg command line use easy for common use cases

 
Question is if there is a usable subset that would cater to many users' needs. And if so, if it would be possible to find out what that might be. For me (jeorgen) it is:

* conversion (lossless, or close to it)
* sound extraction
* trimming
* slow motion  (with intact pitch if ffmpeg is up to it)
* picture-in-picture (speaker + overhead projector)
* 

## Extract clip
Here are some snippets that I have used, as a starting point. In 2010, these snippets worked to extract a clip from a video file:

    ffmpeg -sameq -ss 00:02:00 -t 8 -i long.mov short.mov

    ffmpeg -y -i input.avi -ss 30 -t 10 -vcodec copy -acodec copy output.avi

    mencoder -ss 1284 -endpos 102 movie.avi -oac copy -ovc copy -o movie-clip.av


##Picture in picture (pip) video with ffmpeg

The following command line worked on Ubuntu 13.10. However it does not work with the avconv/ffmpeg shipped with Ubuntu. Instead a static build was used from this site:

http://ffmpeg.gusari.org/static/

     ~/bin/ffmpeg -i inlay.mkv -i background.mp4 -filter_complex "[0]scale=iw/5:ih/5 [pip]; [1][pip] overlay=main_w-overlay_w-10:main_h-overlay_h-10" PIP_video.mp4

This part:

    [0]scale=iw/5:ih/5

...seems to control the size of the inlay, so a bigger divisor yields a smaller inlay. It may be that the [pip] follwing the part, tags file 0 as a pip thingy.

 

This part:

    overlay=main_w-overlay_w-10:main_h-overlay_h-10

...seems to control where the inlay is placed, with coordinates being x,y with origo in the top left of the background (main) video. "main_w", "main_h", "overlay_w" and "overlay_h" seems to be variables available denoting the width and height of each video.

It may be that the "[1][pip]" preceding it first refers to the background video (being indexed as 1, that is the second video in a 0-based system), and the the "[pip]" somehow carries over from the preceding part and then references the video there. "pip" may have some pippy meaning or it is just a tag.

##In 2012:  How to make an mp3 file of a Youtube flv video with aac sound

On Linux, using ffmpeg with lame installed:

ffmpeg -i video.flv -acodec libmp3lame -aq 2 audio.mp3

##Converting from a Canon HD video camera, mts format:
    ffmpeg -deinterlace  -i canonvideo.mts -sameq video.mp4


