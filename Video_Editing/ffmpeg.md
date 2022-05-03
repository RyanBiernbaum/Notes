# ffmpeg

## Convert video file to RGB PNG sequence

<details open><summary>Bash</summary>

```bash
ffmpeg -hide_banner -stats -loglevel warning -y \
-i 'in.mp4' \
-compression_level 3 -pix_fmt rgb24 -vsync 0 -r 24000/1001 -frame_pts 1 \
-vf 'pad=width=ceil(iw/2)*2:height=ceil(ih/2)*2:color=black@' \
'out_%9d.png'
```

</details>

<details><summary>PowerShell</summary>

```powershell
ffmpeg -hide_banner -stats -loglevel warning -y `
-i 'in.mp4' `
-compression_level 3 -pix_fmt rgb24 -vsync 0 -r 24000/1001 -frame_pts 1 `
-vf 'pad=width=ceil(iw/2)*2:height=ceil(ih/2)*2:color=black@' `
'out_%9d.png'
```

</details>

## Convert RGB PNG sequence to 8-bit H.264 video using libx264 (software)

<details open><summary>Bash</summary>

```bash
ffmpeg -hide_banner -stats -loglevel error -y -vsync 0 -r 2997/50 \
-i 'in_%9d.png' \
-c:v libx264 -crf 20 -preset medium -pix_fmt yuv420p -movflags +faststart -aspect 16:9 -threads 0 \
'out.mp4'
```

</details>

<details><summary>PowerShell</summary>

```powershell
ffmpeg -hide_banner -stats -loglevel error -y -vsync 0 -r 2997/50 `
-i 'in_%9d.png' `
-c:v libx264 -crf 20 -preset medium -pix_fmt yuv420p -movflags +faststart -aspect 16:9 -threads 0 `
'out.mp4'
```

</details>

___

[Back](./README.md)  
[Home](../README.md)  
