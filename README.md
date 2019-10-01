### Attention: Youtube-dl works for the site now, making this script almost irrelevant.

I'll try to make it get a few variables that youtube-dl doesn't currently get.

### Fixing youtube-dl extractor for better compatibility with Iwara

- Update youtube-dl
- Download this version of the [iwara extractor.](https://raw.githubusercontent.com/ytdl-org/youtube-dl/6b31024945dcd89ce27c4e8706d9cd1cb40f6c12/youtube_dl/extractor/iwara.py)
- Make a backup of the original extractor (just in case)
- Replace old extractor with the downloaded version
- ???
- Profit!!

The new extractor supports:

- title
- age_limit
- formats
- upload_date
- uploader
- description
- comment_count
- like_count
- view_count

# Iwachan
Script to play videos from Iwara without the embedded player

This script is based on the tv-player script before the site updated to it's now Iwara design/state.

Make the script executable and place it in either your $HOME/bin or $PATH.

#### Current Version: Iwachan-v1.04

### Requirements:
	- mpv with networking support(ffmpeg with networking)
	- everything else should be on a default GNU/Linux install

### Uses:
	
If the script is in your current directory:

	- ./iwachan URL
	- ./iwachan -d URL
	- ./iwachan -h
If the script is in your $PATH:

	- iwachan URL
	- iwachan -d URL
	- iwachan -h

### Errors:

	- iwachan -i URL

Currently, the script will probably not give you much information if something goes wrong.
The -i flag will either fail, get the wrong information, or work seamlessly. This is due to the way the page is formatted in html. I will see if I can fix this with just 
the current methods I'm using. If I can't, I'll have to look into an html parser of some kind.

### To-Do:
I'm not entirely sure of how the site functions yet. There is bound to be some errors when trying to run the script. I will get around to fixing these later on as I find them.

There are some videos that used to be hosted by third-party sites that were just embedded into the previous (trollvids) site. I'm not sure how these videos are currently hosted.
As time goes on, I will try to see if I can get them working based on how Sita makes his site work.

There are private videos on the new site as well. I don't know too much on how they work, so I will try a few things out. Don't get your hopes up.

	- Google hosted videos
	- Youtube hosted videos
	- Externally hosted videos
	- Private videos

Any comments, questions, or jokes are appreciated.
