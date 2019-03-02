# Comical
A python script to convert an MPEG-2 Transport Stream (TS) into a .pdf 'graphic novel'

Prepared for MagPi #80 [https://raspberrypi.org/magpi/]()

## Installation

```bash
$ cd
$ sudo apt install ccextractor ffmpeg imagemagick tesseract
$ pip3 install fpdf
$ git clone https://github.org/mrpjevans/comical.git
$ cd comical
```

## Usage

The script has several stages than can be run individually, all at once, or all steps up the final
generation of the PDF. These steps are:

- Extracting the subtitle images
- Filtering the images to make suitable for OCRing
- OCRing the images to create text
- Extracting still frames from the video based on the subtitle times
- Detecting scene changes in the video
- Extracting images at the point of scene changes
- Compiling all the images and subtitles into a PDF

Available switches:

-i, --input
REQUIRED. The absolute or relative path to the .ts file.

-o, --output
REQUIRED. The output file (PDF) to be generated.

-f, --full
Complete all steps in a single run.

-p, --prebuild
Complete all steps except for the final PDF build.

-e, --extract
Extract the subtitle stream and convert to XML/PNG.

-c, --clean
Take the raw PNG images and resize then convert to grayscale to improve OCR.

-r, --ocr
Peform optical character recognition on the PNGs and output a text file for each.

-m, --images
Extract a still frame in JPEG format at the timecode for each subtitle.

-d, --detectscenes
Analyse the video and create a list of timecodes for all significant scene changes.

-s, --extractscenes
Based on the output of the previous step, extract still frames for scene changes.

-b, --build
Take all the images and text files and compile into a single PDF.

## Notes On Operation

To use, get a .ts file, such as one recorded from the Raspberry Pi DVB HAT. We'll call
ours example.ts.

The script uses ccextractor to get the subtitles out as PNGs. This will create a
directory based on the input file name and '.d'. So, ours would be 'example.d'. At the
working directory level, an XML file will be created detailing the timestamps for each
subtitle.

Another directory, example_process will be created in subsequent stages to collect all
the images and text files requied for the PDF. Filenames will represent their timestamp.

Once the PDF has been created, both directories can be deleted.

## Examples

To do a full start-to-finish conversion:

```bash
$ python3 comical.py -i example.ts -o example.pdf --full
```

To do everything except build the PDF:

```bash
$ python3 comical.py -i example.ts -o example.pdf --prebuild
```

Why would you do this? If you have some lead-in and lead-out from a recording, this
gives you opportunity to remove unwanted images and subtitles from the _process folder.
You can then create the PDF with:

```bash
$ python3 comical.py -i example.ts -o example.pdf --build
```

## Finally

This script is just a bit of fun and absolutely done 'because I can'. PRs welcome if you
see room for improvement!