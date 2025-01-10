# FFmpeg

A complete, cross-platform solution to record, convert and stream audio and video.

## Links

- [FFmpeg](https://ffmpeg.org/)
- [YouTube (Fireship) - FFmpeg in 100 Seconds](https://www.youtube.com/watch?v=26Mayv5JPz0)

## Utils

```sh
# Combine 2 videos one after the other
ffmpeg -i video1.mp4 -i video2.mp4 -filter_complex "[0:v] [0:a] [1:v] [1:a] concat=n=2:v=1:a=1 [v] [a]" -map "[v]" -map "[a]" output.mp4

# Add music to a video
ffmpeg -i video.mp4 -i music.mp3 -filter_complex "[0:a] [1:a] amerge=inputs=2 [a]" -map 0:v -map "[a]" -c:v copy -c:a aac -ac 2 -shortest output.mp4
```
