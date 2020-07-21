## Download
Bash/Linux:
```bash
youtube-dl -f bestaudio  "https://www.youtube.com/playlist?list=PLYRruMbyFRcBVdVN8v4FNkIKkXvL-bZn_" --exec "ffmpeg -i {}  -codec:a libmp3lame -qscale:a 0 {}.mp3 && rm {} "
```

Windows audio only: 
```bash
youtube-dl -f bestaudio  "https://www.youtube.com/watch?v=zgOTgPrd2kU" --exec "ffmpeg -i {}  -codec:a libmp3lame -qscale:a 0 {}.mp3 && del {} "
```

Windows audio+video:
```bash
youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/mp4' https://www.youtube.com/watch?v=sLgEVkIkkQ8
```

FFMPEG pure:
```bash
ffmpeg -i input.mkv -c copy output.m4v
```

## Reddit
Search reddit for exact stuff:

https://camas.github.io/reddit-search/#%7B%22subreddit%22:%22India%22,%22resultSize%22:1000,%22query%22:%22Jain%22%7D

### Download Subreddit
https://github.com/libertysoft3/reddit-html-archiver

#### Fetchind data
data is fetched by subreddit and date range and is stored as csv files in data.
`./fetch_links.py politics 2017-1-1 2017-2-1`

#### Or add some link/post filtering to download less data
`./fetch_links.py --self_only --score "> 10" politics 2005-1-1 2030-1-1`

#### Show available filters
`./fetch_links.py -h`

#### Write
Write html files for all subreddits to r: `./write_html.py`

#### Or add some output filtering for less fluff or a smaller archive size
`./write_html.py --min-score 100 --min-comments 100 --hide-deleted-comments`

#### Show available filters
`./write_html.py -h`

### Download images from links inside html files in the folder
https://github.com/ruanchaves/reddit-html-archiver-image-plugin

## wget
```bash
wget --recursive --no-clobber --page-requisites --html-extension --convert-links --restrict-file-names=windows --domains abhinavkr.ga --no-parent https://abhinavkr.com
```

Or simplified:

```bash
$ wget \
     --recursive \
     --no-clobber \
     --page-requisites \
     --html-extension \
     --convert-links \
     --restrict-file-names=windows \
     --domains website.org \
     --no-parent \
         www.website.org/tutorials/html/
```
* `--recursive`: download the entire Web site.
* `--domains website.org`: don't follow links outside website.org.
* `--no-parent`: don't follow links outside the directory tutorials/html/.
* `--page-requisites`: get all the elements that compose the page (images, CSS and so on).
* `--html-extension`: save files with the .html extension.
* `--convert-links`: convert links so that they work locally, off-line.
* `--restrict-file-names=windows`: modify filenames so that they will work in Windows as well.
* `--no-clobber`: don't overwrite any existing files (used in case the download is interrupted and resumed).

# Links for piracy
https://github.com/Igglybuff/awesome-piracy

# Watch YouTube faster
Enter this in console:
```js
document.getElementsByTagName("video")[0].playbackRate = 2.5;
```

# VsCode
* **Open a folder**: Open cmd in the folder and type `code .`
* **Open a file in code**: Type in cmd `code whatever.htm`
* **Open a file in project by name**: Ctrl+P
* **Go to prev/next file**: Left alt + left/right arrow keys
* **Move lines up/down**: Left alt + up/down keys

