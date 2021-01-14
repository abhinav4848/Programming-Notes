## Download Vids via Youtube-dl
**Bash/Linux:**
```bash
youtube-dl -f bestaudio  "https://www.youtube.com/playlist?list=PLYRruMbyFRcBVdVN8v4FNkIKkXvL-bZn_" --exec "ffmpeg -i {}  -codec:a libmp3lame -qscale:a 0 {}.mp3 && rm {} "
```

**Windows audio only:**
```bash
youtube-dl -f bestaudio  "https://www.youtube.com/watch?v=zgOTgPrd2kU" --exec "ffmpeg -i {}  -codec:a libmp3lame -qscale:a 0 {}.mp3 && del {} "
```

**Windows audio+video:**
```bash
youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/mp4' https://www.youtube.com/watch?v=dPuzkFfAESI
```
Change `ext=mp4` to webm if you want to push for 8k video. `m4a` is the best audio available. `mp4` after that is the final format to merge the two files and save in. Still, Higher res files will be saved as mkv if necessary.


***More complex youtube-dl***

```
youtube-dl -ciw --add-metadata --download-archive doneVids.txt -o '%(title)s-%(id)s.%(ext)s' -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/mp4' 9OYxETIV8bM
```

Explanation:
```
youtube-dl
-ciw
	-c, --continue  [Force resume of partially downloaded files]
	-i, --ignore-errors [Continue on download errors, for example to skip unavailable videos in a playlist]
	-w, --no-overwrites  [Do not overwrite files]

--write-description 
--write-annotations 
--write-thumbnail 

--download-archive done.txt
	writes a text file with the video id so that it's not redownloaded again

-o 
	Output file name
		'UpID-%(uploader_id)s 
		Date-%(upload_date)s 
		Title-%(title)s 
		ID-%(id)s.%(ext)s' 

-f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/bestvideo+bestaudio' --merge-output-format mp4 

Video ID: 9OYxETIV8bM
```

**FFMPEG pure:**
```bash
ffmpeg -i input.mkv -c copy output.m4v
```

### Update Youtube-dl
```pip3 install --upgrade youtube-dl```

## Reddit
**Search reddit for exact stuff:**

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

# Twitter advanced Search
Search twitter for advanced stuff:

https://twitter.com/search-advanced

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

# Get access to a video file's url in a website using xpath
Find the url once by inspect element. Then copy the full xpath of that element.
Open console and enter:
```js
function getElementByXpath(path) {
  return document.evaluate(path, document, null, XPathResult.FIRST_ORDERED_NODE_TYPE, null).singleNodeValue;
}
```
then enter
```js
console.log( getElementByXpath("//html[1]/body[1]/div[1]") );
```
[source](https://stackoverflow.com/a/14284815/2365231)

# VsCode
* **Open a folder**: Open cmd in the folder and type `code .`
* **Open a file in code**: Type in cmd `code whatever.htm`
* **Open a file in project by name**: Ctrl+P
* **Go to prev/next file**: Left alt + left/right arrow keys
* **Move lines up/down**: Left alt + up/down keys

# Google Search Operators
```
"exact query"
website.com: query
query 2017..2018
query ..2017 (so only 2017)
filetype:pdf query
query -excludedTerm
link:nytimes.com (to find pages that link here)
“Come * right now * me” (* is a wildcard placeholder that google will fill)
related:amazon.com
chocolate OR white chocolate
```

