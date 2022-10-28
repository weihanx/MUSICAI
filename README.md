# Multitrack Music Transformer

## Prerequisites

### Set up development environment

We recommend using Conda. You can create the environment with the following command.

```sh
conda env create -f environment.yml
```

## Preprocessing

### Download the datasets

- [Lakh MIDI Dataset (LMD)](https://qsdfo.github.io/LOP/database.html)
  - Download in command line directly via `wget http://hog.ee.columbia.edu/craffel/lmd/lmd_full.tar.gz`
- [Symbolic orchestral database (SOD)](https://qsdfo.github.io/LOP/database.html)
  - Download in command line directly via `wget https://qsdfo.github.io/LOP/database/SOD.zip`
- [SymphonyNet Dataset](https://symphonynet.github.io/)
  - Download in command line directly via `gdown https://drive.google.com/u/0/uc?id=1j9Pvtzaq8k_QIPs8e2ikvCR-BusPluTb&export=download`

### Prepare the name list

Get a list of filenames for each dataset.

```sh
find data/sod/SOD -type f -name *.mid -o -name *.xml | cut -c 14- > data/sod/original-names.txt
```

> Note: Change the number in the cut command for different datasets.

### Convert the data

Convert the MIDI and MusicXML files into MusPy files for processing.

```sh
python convert_sod.py
```

> Note: You may enable multiprocessing via the `-j {JOBS}` option. For example, `python convert_sod.py -j 10` will run the script with 10 jobs.

### Extract the note list

Extract a list of notes from the MusPy JSON files.

```sh
python extract.py -d sod
```

> Note: Pass the option `-d lmd` instead for the LMD dataset.

### Split training/validation/test sets

Split the processed data into training, validation and test sets.

```sh
python split.py -d sod
```

## Training