# list2mp3 : A Universal Audio Merger

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ras0k/list2mp3/blob/main/list2mp3.ipynb)

A simple tool for downloading audio from one or more YouTube  playlists and/or videos and merging them into a single MP3 file. This project uses [yt-dlp](https://github.com/yt-dlp/yt-dlp) for downloading and [ffmpeg](https://ffmpeg.org/) for merging audio files. The interface is built using [ipywidgets](https://ipywidgets.readthedocs.io/) to provide a user-friendly experience within a Jupyter Notebook or Google Colab.

## Features

- **Download Audio:**  
  Extracts audio from individual YouTube videos or entire playlists.

- **Auto-Naming:**  
  Automatically determines the output file name based on the playlist title. The title is sanitized to avoid any filename issues.

- **Merge Audio:**  
  Downloads audio tracks from multiple URLs and merges them into a single MP3 file using ffmpeg.
    
-   **URL Transformation:**  
    Automatically transforms YouTube watch URLs that include a playlist parameter into proper playlist URLs.
    
-   **Metadata Integration with Cropped Cover Art:**  
    Extracts metadata from the first video in a playlist to set the MP3 title and artist tag. Downloads the first video’s thumbnail, crops it to a centered square (losing the sides), and embeds it as the cover art.

- **Interactive UI:**  
  Uses ipywidgets to offer a simple, interactive interface in a Jupyter Notebook or Google Colab.

- **Verbose Logging:**  
  Provides detailed status messages during the download and merge process to help diagnose issues.

-   **Folder Cleanup:**  
    Clears out previous temporary files (audio files, file lists, cover images) at the start so the notebook can be run multiple times without interference.

## Get Started on Google Colab

To try out the YouTube Audio Merger tool without any local setup, open the notebook on Google Colab using the link below:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ras0k/list2mp3/blob/main/list2mp3.ipynb)

## How It Works

- **yt-dlp:**
  
  Downloads the audio from the provided URLs using:
  ```bash
  yt-dlp -x --audio-format mp3 -o '%(playlist_index)s_%(title)s.%(ext)s' <URL>
  ```

- **ffmpeg:**
  
  Merges the downloaded MP3 files using a generated file list:

  ```bash
  ffmpeg -f concat -safe 0 -i file_list.txt -c copy 'merged_audio.mp3'
  ```

  Crops the downloaded thumbnail to a centered square:
  ```bash
  ffmpeg -y -i cover.jpg -vf "crop=min(iw,ih):min(iw,ih):(iw-min(iw,ih))/2:(ih-min(iw,ih))/2" cropped_cover.jpg
  ```
  Embeds the cropped thumbnail along with metadata:
  ```bash
  ffmpeg -i "merged_audio.mp3" -i cropped_cover.jpg -map 0 -map 1 -c copy -id3v2_version 3 
        
  -metadata title="<Output Name>" -metadata artist="<Channel Name>" 
        
  -metadata:s:v title="Album cover" -metadata:s:v comment="Cover (front)" "temp_<Output Name>.mp3"
  ```
-   **Playlist Title Sanitization:**
    
    The tool extracts and sanitizes the playlist title to generate a safe output file name.

## Changelog

### v2.2 (2025-03-09)

- **Album Cover Improvement:**
Checks all the thumbnails and chooses the most frequent one (for cases like when the first song has a music video)

### v2.1 (2025-03-01)

- **Artist Name Improvement:** Removes " - Topic" from the end of the artist name.

### v2.0 (2025-02-24)

-   **Folder Cleanup:** Added a function to clear temporary files before each run.
    
-   **URL Transformation:** Automatically converts YouTube watch URLs with a playlist parameter into proper playlist URLs.
    
-   **Improved Metadata Integration:**
    
    -   Extracts metadata from the first video in a playlist using `--playlist-items 1`.
        
    -   Downloads the first video's thumbnail.
        
    -   Crops the thumbnail to a centered square using an ffmpeg crop filter (ensuring the cover art does not add black bars).
        
    -   Embeds the cropped thumbnail as cover art along with updated MP3 metadata (title and artist).
        
-   **Enhanced Logging:** Verbose output now indicates each step including cleanup, URL transformation, and thumbnail processing.


    
## Contributing

Contributions are welcome! If you have ideas, bug fixes, or enhancements, feel free to open an issue or submit a pull request.

#### *TO-DO:*

- *Further improvements to metadata extraction*
- *Additional customization options for audio quality and output settings*
- *Automatic chapters should be added to metadata, similar to what [Seal](https://github.com/JunkFood02/Seal) does with YouTube chapters, but with each video as one chapter instead. This change would probably require using .opus format instead of .mp3*

## License

This project is licensed under the MIT License. See the LICENSE file for details.
