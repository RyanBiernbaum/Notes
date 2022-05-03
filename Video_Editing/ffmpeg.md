# ffmpeg

## Convert video file to RGB PNG sequence

    ffmpeg -hide_banner -stats -loglevel warning -y \
    -i in.mp4 \
    -compression_level 3 -pix_fmt rgb24 -vsync 0 -r 24000/1001 -frame_pts 1 \
    -vf pad=width=ceil(iw/2)*2:height=ceil(ih/2)*2:color=black@ \
    "out_%9d.png"

# Convert RGB PNG sequence to 8-bit H.264 video using lobx264 (software)



___

[Back](README.md)  
[Home](../README.md)  
