# Noritsu EZController scanned images cleanup
TODO: fill out examples of settings to use in EZController

## What the problem is
When using a Noritsu scanner with EZController, the images that it outputs have some missing EXIF data. Specifically, the files are missing the DateTimeOriginal and related EXIF tags. This means that any image-editing software will use the last modified date from the filesystem as the "capture date," meaning rotating images or simply renaming images causes images to jump forward in time. This usually translates to Lightroom missing up the order of the photos by the time they are imported.

## What this script does
The script does 3 main things:
 1. Assigns DateTimeOriginal to each image, using the first image's modified time, then adding 1 millisecond for each subsequent image. This preserves the sorting order in the same order they were scanned. Essentially, it's as if all the images were scanned in quick succession, 1 millisecond apart.
 2. Deletes the Info_HD.txt file if it exists.
 3. Renames the images from `000074660003_1.jpg` to `R7466F3.jpg`, a simplification into this format: `R{roll_number}F{frame_number}.jpg`.

## Installation
TODO: update this when the wheel is easier to find/download.
The script was written in Python 3.9, but likely works for Python 3.6 and newer. To install, use either `pip` or `pipx` to install the `frontier-scans-cleanup` package.

*[ExifTool](https://exiftool.org/install.html)* must both be installed for the script to work. For MacOS users, you can alternatively use Homebrew to install it: `brew install exiftool`.

## Run
Run `noritsu-clean` in the directory you wish to search for Noritsu scans from. Alternatively, you can specify the location as an argument: `noritsu-clean 20211226/00007466/`.
## Optional flags
By default, the script will not keep the frame number from the DX reader (in the filename) in the renamed files, and instead use an auto-incrementing sequence number starting at 01. If you would like to keep the original frame numbers, add the `--use_frame_names` argument, but keep in mind this may cause image to overwrite if multiple images have the same frame number (such as when the DX reader can't detect frames, or if the film has no rebate).
