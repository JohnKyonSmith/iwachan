# Iwachan
Script to download videos from Iwara/External sites using youtube-dl.
This is a complete redo of the original iwachan script.

#### Current Version: Iwachan-v2.1

## Description:
Helper script to use __youtube-dl__ as __iwachan__. It detects if the URL is from Iwara, an external site (Ex. Youtube), or a batch file. If it detects a batch file, it separates the links to use different youtube-dl settings. It will get the __best source__ from Iwara and __1080p60fps or lower__ from external sites. You need to manually edit these settings if you want other video/audio resolutions/formats (_look into the __youtube-dl -F__ option_). It uses two external files (__iwachan__ and __iwachan-hq__) that have the custom settings in the $HOME/.config/youtube-dl directory. The __iwachan file__ has the settings for __iwara.tv__ while __iwachan-hq__ has the settings for __external sites__. Lastly, this script uses the __updated extractor__.

## Requirements:
 - youtube-dl (with updated extractor)
 - youtube-dl configs (iwachan & iwachan-hq)
 - ffmpeg
 - xsel
 - everything else should be on a default GNU/Linux install

## Installation
You must install all the files in their respective locations. Adjust the paths if you need to. Install __xsel__ from your package manager. Install __youtube-dl__ from your package manager or from python-pip. Make the script __executable__ and place it in either your __$HOME/bin__ or __$PATH__. These commands will clone this repo, make a backup of the original iwara.py extractor, make symlinks to where all the files need to go, and symlink the script to your $HOME/bin directory.
```
git clone https://github.com/JohnKyonSmith/iwachan.git && cd iwachan
mkdir -p $HOME/.config/youtube-dl
chmod +x iwachan
cp $HOME/.local/lib/python3.7/site-packages/youtube_dl/extractor/iwara.py extractor/iwara.py.orig
ln -sf $(pwd)/extractor/iwara.py $HOME/.local/lib/python3.7/site-packages/youtube_dl/extractor/iwara.py
ln -sf $(pwd)/config/iwachan $HOME/.config/youtube-dl/iwachan && ln -sf $(pwd)/config/iwachan-hq $HOME/.config/youtube-dl/iwachan-hq
ln -sf $(pwd)/iwachan $HOME/bin/iwachan
```

## Information:
### Fixing youtube-dl extractor after updating youtube-dl
If you get the original extractor after updating youtube-dl, go back to the cloned repository and run this command again.
```
ln -sf $(pwd)/extractor/iwara.py $HOME/.local/lib/python3.7/site-packages/youtube_dl/extractor/iwara.py
```

### Updated Extractor
The new extractor supports:

 - title
 - age\_limit
 - formats
 - upload\_date
 - uploader
 - description
 - comment\_count (causing errors, removed)
 - like\_count
 - view\_count

Credit to __AlexAplin__ for the updated extractor.

### Youtube-dl config files
If you want to change the output directory/formats/other settings, edit the __iwachan & iwachan-hq__ config files in the iwachan/config directory, and symlink them again from the root of the cloned repo with the following command:
```
ln -sf $(pwd)/config/iwachan $HOME/.config/youtube-dl/iwachan && ln -sf $(pwd)/config/iwachan-hq $HOME/.config/youtube-dl/iwachan-hq
```
### Youtube-dl config file settings
__-f__ option: This is the formats option. This chooses the quality/format of the video/audio you want to download. The settings for the iwachan file is quite simple. The settings for the external sites are a little more complicated. _Read the youtube-dl manpage to understand what exactly is going on._

__--output=__ option: This decides where to put the downloaded videos and how to name the files. It must have quotes in order to accept spaces. The accepted values are the ones lsted in the "Updated Extractor" section. _Look at the youtube-dl manpage for more info on how to use the values._

__--ignore-errors__ option: Prevents youtube-dl from exitting due to dead links or other minor errors.

__--write-sub/--sub-lang en__ options: Download subtitles in specified language if available.

__--merge-output-format=mkv__ option: Merge the downloaded files into the mkv container. This is mostly for external sites like Youtube that use separate video and audio files for the higher resolution videos. Not applicable to Iwara since there are no separate audio and video files.

__The rest of the options.__ The rest of the options are mostly for personal use, but they shouldn't affect anything for most people. _Look at the youtube-dl manpage for information on what these options do._

### Output
The output files will be in your specified directories based on the youtube-dl config settings files _(see above)_

__(This is the default)__
```
$HOME/Videos/MMD/SITE/USER/[UPLOAD_DATE] VIDEO_TITLE [URL_ID].EXTENSION
or
$HOME/Videos/MMD/Iwara/TestUser/[20200128] Cool MMD [XXXXXXX].mp4
```
I believe all files from __Iwara__ are __mp4__ while __external sites__ get muxed to __mkv__ since it's getting the best __1080p60__ video and best audio, usually __160k opus__ for youtube.

## Uses:

```
iwachan             (Uses the latest entry in your clipboard using the "xsel" program)
iwachan URL         (Uses the link you pass to the script)
iwachan batch.txt   (Uses a plain text batch file)
```

## Batch Files:
The batch file is a plain text file with one link per line

```
cat batch.txt
https://www.youtube.com/watch?v=URL_ID
https://www.youtube.com/watch?v=URL_ID
https://ecchi.iwara.tv/videos/URL_ID
https://iwara.tv/videos/URL_ID
```

## Errors:
### FIXED clipboard sanitation for Iwara links in v2.1
All iwara.tv links must be plain text links without anything after the URL\_ID or the extractor will throw an error and either refuse to download, download lower quality videos, or not properly put the videos in their directories.

__VALID__ link with expected output
```
https://ecchi.iwara.tv/videos/URL_ID
```
__INVALID__ link that will throw an error
```
https://ecchi.iwara.tv/videos/URL_ID?language=en
```
## Solutions:
### FIXED clipboard sanitation for Iwara links in v2.1
Either browse the site in your preferred language and __manually remove the extra information__, browse the site __in the native japanese language__, or put all the links in a batch file and run the __clean\_batch function__ to clean the links automatically. _Information about the __clean\_batch__ function and how to use it is below_.

## Notes:

__ONLY FOR iwara.tv/youtube.com links, NOT shortened links.__
The clean\_batch function will clean the batch file you input. It'll look for files (excluding .part files) that have a matching URL\_ID and delete the links from the batch file. It'll also remove anything after the first ? (question mark) in iwara links so as to not confuse the youtube-dl extractor. __The input to run the function _must_ have CLEAN then the batch file.__
```
iwachan CLEAN batch.txt
```
This will create a backup of the batch file (batch.txt.bak) in case there is an error or you want to manually review what it removed __(The old backup file will be overwritten everytime you run the clean_batch function, so be careful!)__, then just run the script as usual, pointing to the batch file.
```
iwachan batch.txt
```

## To do:
 - Fix any bugs that crop up
 - Try to make the script more efficient
 - Deal with third-party hosted videos within Iwara
 - Sanitize Iwara links to remove anything after the URL\_ID from the clipboard (batch files still need to use the CLEAN\_BATCH function)

## Fix me:
 - Private videos

I don't know too much on how they work, so I will try a few things out. Don't get your hopes up.

Any comments, questions, or jokes are appreciated.
