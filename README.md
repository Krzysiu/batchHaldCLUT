# batchHaldCLUT
*"Imagine all the images processing in baaatchhh, yeah. You may say I'm dreamer, but now I've got a tool. I hope someday you'll clone it and many operations will become one."*

Batch create files with RawTherapee using all possible HaldCLUT presets. While it uses PHP, I'm trying to make it simple for users who don't know this language.

## Requirements 
* **Windows** - just for now; soon it will work with other systems
* **RawTherapee 5.x** {[Download](https://rawtherapee.com/downloads)} - ealier versions not tested, but might work
* **CLUT files** {[Download (402 MB!)](https://rawtherapee.com/shared/HaldCLUT.zip)}
* **PHP 7.x** {[Download for Windows](https://windows.php.net/download); other OS: use package manager} - 5.x would possibly work, but it's not tested
* **.COM and NET** extension - available in the default PHP package; script tries to turn it on, if it's not enabled; if it fails, open `php.ini` from PHP directory and remove semicolon (`;`) before `extension=php_com_dotnet.dll` line.
* **Photo** with RawTherapee **PP3 profile with Film simulation turned on** (click on the power icon near *Film simulation* in *Colors* card); the HaldCLUT directory doesn't have to be set in RawTherapee.

## Example output
Output for 33 HaldCLUT files (Creative pack only).

### Image output
Only images were created by script; the collage is made with ImageMagick (command is below); next version will support automatic creation of such all-in-one-with-nice-titles-and-generally-awesome images.

![Collage of images made by BatchCLUT](https://i.imgur.com/AzgNneJ.jpg)

<sup>[Image page on imgur](https://imgur.com/a/3xkEyOH)<br>Command: `magick montage -limit memory 256MiB -limit map 256MiB -define jpeg:size=350x233 -label %t -tile 6x -frame 1 -geometry 350x233+1+1 *.jpg out.png` - it takes all .jpg files, resizes it, labels it with the file name, adds margins between them, renders them as tiles (6x = 6 rows and as much columns as possible) and won't let your PC hang because memory limits</sup>

### Console output
```
Processing 33 HaldCLUT profiles. JPEG quality: 75% (chroma 4:2:2 (balanced)), fast export disabled.
Files will be saved to: E:\cluttest\out

Processing 1/33... Done! Operation took 6s (average: 6.0s). Estimated time to complete: 03m 12s
Processing 2/33... Done! Operation took 5s (average: 5.5s). Estimated time to complete: 02m 50s
[...]
Processing 32/33... Done! Operation took 6s (average: 5.7s). Estimated time to complete: 00m 05s
Processing 33/33... Done! The operation took in total 03m 07s
```
## Config
**Path note:** script takes care for quoting path, so for `c:\foo bar` you don't need to write `"\"c:\\foo bar\""`, but simply `"c:\\foo bar"`. Trailing slash shouldn't matter.

* `rawThreapeeBinary` <sup>*string*</sup> - the full path (including file name) to RawTherapee CLI (rawtherapee-cli.exe in Windows)
* `inputPhoto` <sup>*string*</sup> - the full path to the photo
* `inputProfile` <sup>*nullable string*</sup> - input profile; if `null`, use default profile name (file name + `.pp3`)
* `outputDirectory` <sup>*string*</sup> - directory for output files; if doesn't exist, it will be created
* `outputJPEGQuality` <sup>*integer*</sup> - output JPEG quality from 1 to 100
* `outputJPEGChroma` <sup>*integer*</sup> - JPEG chroma subsampling - `1` for 4:2:0 (smallest file); `2` for 4:2:2 (balanced); `3` for 4:4:4 (best quality). This setting doesn't affect colors *per se*, but rather color changes - for files with sharp, colorful edges, it's better to use `3` setting
* `outputFastExport` <sup>*boolean*</sup> - `true` for fast export, i.e. very fast export which bypasses some filters; it should be used only for previews, so in many cases it could be useful here
* `clutDirectory` <sup>*string*</sup> - directory with CLUT files. The script uses recursive method, i.e. it will get all files from given directory, but also all files from subdirectories.

## To do
* Conditional execution method, which would turn script into OS-independent
* Allow running more than one instance at once (remove constant temporary file name)
* Remove temporary files after processing
* Turn on *Film simulation* if profile says it's disabled
* Process photos without profiles
* Downsizing of results
* Optional creation of collage instead single files
* Custom name schemas
* Custom strength of filter with ability to provide array of strengths to produce multiple versions
* Allow overwritting files or inform if the image was ignored due to non-overwrite policy
* Move config to separate file
