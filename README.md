# videocutter

[![Python Version](https://img.shields.io/pypi/pyversions/videocutter.svg)](https://pypi.org/project/videocutter)
[![PyPI](https://img.shields.io/pypi/v/videocutter.svg)](https://pypi.org/project/videocutter/)

This script is used to cut long recordings containing multiple stimulations into single-stimulation clips.

It uses the video file and a corresponding stimulation trace stored as a text (Labscribe) or binary file (Bonsai)

The script processes all videos found in the input directory that have a txt/csv/bin file with the same name. The latter is read to determine the stimulation onsets. Extracted clips are created in {video-name}-cropped subfolder and are numbered from 0 to the number of stimulations found in the stimulation file.

The stimulation trace files can either be :
- A text/CSV file with two columns, the first one being the time and the second the voltage.
- A binary file saved with Bonsai, with three columns, the first one being the time steps, the second one being the blue laser voltage and the third one the orange laser voltage.

## Install
Within a virtual environment with Python 3.12, install with `pip` from the terminal :  
```bash
pip install videocutter
```

## Usage
### As a command line interface (CLI)
From a terminal within the virtual environment in which you installed `videocutter`, you can check the default values with :
```bash
videocutter --help
```
Then, use it like so :
1. With text files, with all default values
```bash
videocutter /path/to/your/videos
```
2. With Bonsai files, extract blue laser and use all default values
```bash
videocutter /path/to/your/videos blue
```
3. With text files, changing the time before and after onset :
```bash
videocutter /path/to/your/videos --time-before 1 --time-after 2
```
4. With binary files, extract another laser color and change video files extension :
```bash
videocutter /path/to/your/videos orange --video-ext avi
```

### From a Python script
Copy the example from `examples/cut_videos.py`, fill in the parameters and run the script.

## Notes
- The format of txt files exported from Labscribe depends on its version... Sometimes the values are separated by commas (`,`), sometimes tabulations. To be sure, open the file with a text editor and see if there are "," or big spaces between values on a row. Edit the `SEP` parameter accordingly in the example script or with the `--sep` argument in the CLI.
- Again with Labscribe, it's unclear when or which versions creates a header to this text file (names for each columns). The two should work : if there are non-numeric values on the first line of the file, it will be considered as a header and discarded.
- If the stimulation trace was edited in Labscribe (for example, if you annotated the traces), the exported file might contain a third column and it will most likely not work.

## Credits
`videocutter` has been primarly developed by [Guillaume Le Goc](https://legoc.fr) in [Julien Bouvier's lab](https://www.bouvier-lab.com/) at [NeuroPSI](https://neuropsi.cnrs.fr/), with Edwin Gatier's algorithm for trial detection.