# ffmpeg editor

1. Docker container
   
   ```
   docker run -it --rm -u root -v %cd%:/video ubuntu /bin/sh
   ```

2. Install ffmpeg
   
   ```
   apt-get update && apt-get install ffmpeg -y
   ```

3. Copy from 1.30s to 2.00s
   
   ```
   ffmpeg -i input.mp4 -ss 00:01:30 -c copy -t 00:00:30 output.mp4
   ```

4. Speed up: 1 - convert to raw; 2 - increase playback frame rate (-r)
   
   ```
   ffmpeg -i input.mp4 -map 0:v -c:v copy -bsf:v h264_mp4toannexb input.raw.h264
   ffmpeg -fflags +genpts -r 180 -i input.raw.h264 -c:v copy output.mp4
   ```

5. Add subtitles

   subtitles.srt
   ```
   1
   00:00:00,000 --> 00:00:05,000
   After 30 sec...
   ```
   ```
   ffmpeg -i input.mp4 -c:v libx264 -crf 10 -vf subtitles="subtitles.srt:force_style='Fontsize=12,PrimaryColour=&Hffffff&,Outline=0.5'" output.mp4
   ```

6. Concatenate

   list.txt
   ```
   file input1.mp4
   file input2.mp4
   file input3.mp4
   ```
   ```
   ffmpeg -safe 0 -f concat -i list.txt -c copy output.mp4
   ```
