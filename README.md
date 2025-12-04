# HDRplot

## Description

There are at least two good reasons to measure the HDR grade of a hevc/mkv video file.

1. Some remuxes miss the HDR metadata maxCLL/maxFALL. As you are supposed to provide this metadata when encoding, 
you sometimes need to measure them. The function `measure_hdr10_content_light_level` in [awsmfunc](https://github.com/OpusGang/awsmfunc) does that. `HDRplot` comes on top of `measure_hdr10_content_light_level` and therefore provides that functionality.

2. When you want to inject the DoVi/HDR10+ metadata of a web-dl into a HDR remux, you have to check previously that the HDR grades match. One way to do that is to check that the brightness match on screenshots taken from both sources. The other way is to plot the HDR grades of the 2 sources. This is what HDRplot does, based on `measure_hdr10_content_light_level`. For this functionality, an essential feature is the possibility to tonemap a DoVi P5 video file to HDR before drawing the plot. Such a functionality is already implemented in [DoVi_Scripts](https://github.com/R3S3t9999/DoVi_Scripts), but [DoVi_Scripts](https://github.com/R3S3t9999/DoVi_Scripts) is not available for Linux/macOS. `HDRplot` aims at providing a cross-platform alternative within `VapourSynth`.


## Requirements

This script needs a working installation of `VapourSynth` with the `ffms2` indexer. It also requires to have compiled the `vs-placebo` plugin with option `dovi`. Finally, `ffmpeg`, `dovi_tool` and `mediainfo` must be on path.


## Installation

1. Ensure all requirements are installed
2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

### Command Line Interface

Run HDRplot directly from the command line:

```bash
# Basic usage
python HDRplot.py video.mkv

# Custom output identifier
python HDRplot.py video.mkv -i my_video

# Custom title for the plot
python HDRplot.py video.mkv -t "My Custom Title"

# With cropping (all values must be even numbers)
python HDRplot.py video.mkv --left 240 --right 240 --top 20 --bottom 20

# With frame trimming (useful for syncing plots)
python HDRplot.py video.mkv --trim-start 100 --trim-end 50

# Combined options
python HDRplot.py video.mkv -i custom -t "Title" -l 240 -r 240 --top 20 -b 20 --trim-start 100 --trim-end 50

# View all available options
python HDRplot.py --help
```

### Command Line Options

- `path` - Path to the video file (required)
- `-i, --identifier` - Tag for output filenames (default: DEFAULT)
- `-t, --title` - Custom title for the plot
- `-l, --left` - Left crop value in pixels (must be even, default: 0)
- `-r, --right` - Right crop value in pixels (must be even, default: 0)
- `--top` - Top crop value in pixels (must be even, default: 0)
- `-b, --bottom` - Bottom crop value in pixels (must be even, default: 0)
- `--trim-start` - Number of frames to trim at the start (default: 0)
- `--trim-end` - Number of frames to trim at the end (default: 0)

### Python API

You can also use HDRplot as a Python module:

```python
from HDRplot import HDRplot

# Basic usage
HDRplot("video.mkv")

# With options
HDRplot(
    path="video.mkv",
    fileIdentifier="my_video",
    title="Custom Title",
    left=240,
    right=240,
    top=20,
    bottom=20,
    trimStart=100,
    trimEnd=50
)
```

## Output Files

- `lightLevel-<identifier>.json` - Cached CLL/FALL measurements (reused on subsequent runs)
- `HDRplot-<identifier>.png` - Generated plot image


## Features

* Can measure the HDR grades of any HDR10/HDR10+/DoVi hevc/mkv video file.
* In case of a DoVi P5 file, a preliminary tonamap to PQ/HDR10 will be performed.
* Plot the CLL/FALL values of each frame. Compute maxCLL and maxFALL.
* Extract the Master Display Luminance Parameters and the main DoVi parameters and print them on the plot.


## Screenshots

![Barbie-remux](./screenshots/HDRplot-BarbieDisc.png)
![Barbie-webdl](./screenshots/HDRplot-Barbie.png)
<!-- ![Barbie-webdl-L1](./screenshots/HDRplot-BarbieL1.png) -->
