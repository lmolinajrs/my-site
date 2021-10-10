---
title: "Stego"
draft: false
images: []
description: "Steganography Cheatsheet for CTF"
lead: "Steganography Cheatsheet for CTF"
menu:
  docs:
    parent: "Other"
weight: 100
toc: true
---

### steghide
```
# Steghide is a steganography program that hides data in various kinds of image and audio files.

# Only supports these file formats:

JPEG, BMP, WAV and AU

steghide info <file> (displays info about a file whether it has embedded data or not)
steghide extract -sf <file> (extracts embedded data from a file)
```
### stegpy
```
# Stegpy is a program for encoding information in image and audio files through steganography.

# Only supports these file formats:

PNG, BMP, GIF, WebP, WAV (Images in a different format are automatically converted to PNG. Different audio formats are not supported at all)

stegpy <file>
```
### bpstegano
```
# BPStegano is a steganography tool with characteristics like AES-128 encryption of input data, random Least Significant Bit (LSB) pixel encoding and supports plain text (raw string) data hiding

# Only supports these file formats:

pdf, exe, jpg, png, txt, gif, etc.

python stegano.py (run the main panel)
```
### foremost
```
# Foremost is a program that recovers files based on their headers , footers and internal data structures.

# Supports these file formats:

jpg, gif, png, bmp, avi, exe, mpg, wav, riff, wmv, mov, pdf, ole, doc, zip, rar, htm, cpp

foremost -i <file> (extracts data from the given file)
```
### stegsolve
```
# Sometimes there is a message or a text hidden in the image itself and in order to view it you need to apply some color filters or play with the color levels. You can do it with GIMP or Photoshop or any other image editing software but stegsolve made it easier. it’s a small java tool that applies many color filters on images.
```
### strings
```
# Strings is a linux tool that displays printable strings in a file. That simple tool can be very helpful when solving stego challenges. Usually the embedded data is password protected or encrypted and sometimes the password is actaully in the file itself and can be easily viewed by using strings

# Supports these file formats:

All

strings <file>  (displays printable strings in the given file)
```
### exiftool
```
# Sometimes important stuff is hidden in the metadata of the image or the file , exiftool can be very helpful to view the metadata of the files.

# Supports these file formats:

Many

exiftool <file>  (shows the metadata of the given file)
```
### exiv2
```
# A tool similar to exiftool.

# Supports these file formats:

Many

exiv2 <file>  (shows the metadata of the given file)
```
### binwalk
```
# Binwalk is a tool for searching binary files like images and audio files for embedded files and data.

# Supports these file formats:

Many

binwalk <file>  (Displays the embedded data in the given file)
binwalk -e <file>  (Displays and extracts the data from the given file)
```
### zsteg
```
# zsteg is a tool that can detect hidden data in png and bmp files.

# Supports these file formats:

png, bmp

zsteg -a <file> (Runs all the methods on the given file)
zsteg -E <file> (Extracts data from the given payload (example : zsteg -E b4,bgr,msb,xy name.png)
```
### wavsteg
```
# WavSteg is a python3 tool that can hide data and files in wav files and can also extract data from wav files.

# Supports these file formats:

wav

python3 WavSteg.py -r -s <soundfile.wav> -o <outputfile>  (extracts data from a wav sound file and outputs the data into a new file)
```
### sonic visualizer
```
# Sonic visualizer is a tool for viewing and analyzing the contents of audio files, however it can be helpful when dealing with audio steganography. You can reveal hidden shapes in audio files.

# Supports these file formats:

Many

sonic-visualiser <file>
```
### hexeditor
```
# Hexadecimal editor. 

# Supports these file formats:

All

hexeditor <file>
```
### turgen system
```
- Versatile utility that allows you to create your own tapes with software for Atari 8-bit computers.

# Supports these file formats:

WAV

# Use

java -jar turgen.jar

(https://sourceforge.net/projects/turgen/)
```
### rabin2
```
- Binary program info extractor

# Supports these file formats:

ELF 32-bit LSB executable

# Use

rabin2 -I <file> 
rabin2 -e <file> 
rabin2 -s <file>
```