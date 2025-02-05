# list-to-mp3 : A Universal Audio Merger

A simple tool for downloading audio from one or more YouTube videos or playlists and merging them into a single MP3 file. This project uses [yt-dlp](https://github.com/yt-dlp/yt-dlp) for downloading and [ffmpeg](https://ffmpeg.org/) for merging audio files. The interface is built using [ipywidgets](https://ipywidgets.readthedocs.io/) to provide a user-friendly experience within a Jupyter Notebook or Google Colab.

## Features

- **Download Audio:**  
  Extracts audio from individual YouTube videos or entire playlists.

- **Auto-Naming:**  
  Automatically determines the output file name based on the playlist title. The title is sanitized to avoid any filename issues.

- **Merge Audio:**  
  Downloads audio tracks from multiple URLs and merges them into a single MP3 file using ffmpeg.

- **Interactive UI:**  
  Uses ipywidgets to offer a simple, interactive interface in a Jupyter Notebook or Google Colab.

- **Verbose Logging:**  
  Provides detailed status messages during the download and merge process to help diagnose issues.

## Get Started on Google Colab

To try out the YouTube Audio Merger tool without any local setup, open the notebook on Google Colab using the link below:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/your-colab-notebook-id)

*Replace `your-colab-notebook-id` in the URL above with the actual notebook ID or link for your project.*

## How It Works

- **yt-dlp:**  
  Downloads the audio from the provided URLs using:
  ```bash
  yt-dlp -x --audio-format mp3 -o '%(playlist_index)s_%(title)s.%(ext)s' <URL>
