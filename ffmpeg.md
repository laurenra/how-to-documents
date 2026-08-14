# Cut and Splice Video files in ffmpeg
## Losslessly cut an mp4 video into several smaller clips

```
ffmpeg -i Walkthrough-20260807_140349-Meeting\ Recording.mp4 \
-ss 00:14:25 -to 00:21:45 -c copy part-1-Emergency-Building-Coordinator-Portal-EBC-stories.mp4 \
-ss 00:21:46 -to 00:28:45 -c copy part-2-Minor-Protection-App-stories.mp4 \
-ss 00:28:49 -to 00:35:00 -c copy part-3-Litigation-Site-stories.mp4 \
-ss 00:39:15 -to 00:41:10 -c copy part-4-Waiver-Lookup-stories.mp4 \
-ss 00:41:55 -to 00:42:49 -c copy part-5-Incident-Site-and-Waiver-demo-later.mp4 \
-ss 00:42:50 -to 00:52:15 -c copy part-6-Risk-Styles-and-other-stories.mp4
```

## Losslessly combine several mp4 video clips into one

1. Create a text file named **file_list.txt** in the same folder as videos.
2. Add each file path on a separate line using the format: **file 'filename.mp4'**.
3. Save and close the text file.
4. Open the terminal in that folder.
4. Run the command: **ffmpeg -f concat -safe 0 -i file_list.txt -c copy output.mp4**

### file_list.txt

```
file 'part-1-Emergency-Building-Coordinator-Portal-EBC-stories.mp4'
file 'part-2-Minor-Protection-App-stories.mp4'
file 'part-3-Litigation-Site-stories.mp4'
file 'part-4-Waiver-Lookup-stories.mp4'
file 'part-5-Incident-Site-and-Waiver-demo-later.mp4'
file 'part-6-Risk-Styles-and-other-stories.mp4'
```
### Run ffmpeg command to assemble files

```
ffmpeg -f concat -safe 0 -i file_list.txt -c copy output.mp4
```

more stuff...
