# MusicIsAllYouNeed
predict the next track in a spotify playlist using transformers.
This repository contains a series of Jupyter notebooks that process, tokenize, and model a subset of the Spotify 1-Million Playlist Dataset. The workflow is designed to be run in a Google Colab environment and uses data stored in Google Drive (but you can also use it differntly if you prefer).

This guide explains how to run the code. For more detailed information, please refer to the project report.

## Getting Started

To run the code, you'll need access to the Spotify Million Playlist Dataset. The notebooks assume the data is unzipped and located in a specific directory structure on your Google Drive.

### Prerequisites

* A Google account to use Google Colab and Google Drive.
* The unzipped Spotify Million Playlist Dataset stored in `MyDrive/DL_project/dataset/spotify_million_playlist_dataset_unzipped/`.
* you can find it here: [https://engineering.atspotify.com/2018/5/introducing-the-million-playlist-dataset-and-recsys-challenge-2018](https://www.kaggle.com/datasets/himanshuwagh/spotify-million)

---

## Workflow

The project is divided into three main steps, each with its own Jupyter notebook. You must run these notebooks in the following order:

### 1. `pre_process.ipynb`

This notebook prepares the raw JSON data from the Spotify Million Playlist Dataset. It reads the JSON files and converts them into CSV files for easier processing. It handles potential errors gracefully, such as a "file or directory not found" error if the output CSVs don't exist yet.

#### Paths and Parameters
* Change the paths according to your folders:
* **Input Path**: `MyDrive/DL_project/dataset/spotify_million_playlist_dataset_unzipped/`
* **Output Paths**: `MyDrive/DL_project/csv/playlists.csv`, `MyDrive/DL_project/csv/tracks.csv`, `MyDrive/DL_project/csv/merged.csv`

---

### 2. `tokenization.ipynb`

This notebook performs tokenization on the pre-processed data. It uses the merged CSV file to create a unique **`track_key`** by combining the track and artist names. Each unique track key is then assigned an integer ID, creating a vocabulary. The script groups the tracks by playlist to form sequences of track IDs, which are then saved for use in the next step.

#### Paths and Parameters
* Change the paths according to your folders and files:
* **Input Path**: The notebook expects the CSV files created by `pre_process.ipynb` to be in the `csv` directory. Specifically, it uses `tracks_and_playlists.csv`.
* **Output Path**: `MyDrive/DL_project/csv/tracks_with_ids.parquet`
* **Important Parameters**: This notebook's key process is creating a vocabulary and mapping tracks to unique IDs.

---

### 3. `model_and_eval.ipynb`

This final notebook handles the model training and evaluation. It loads the tokenized data and filters it to include only playlists with a minimum number of tracks. The data is then split into training, validation, and testing sets. A subset of the data is used for fast prototyping. The notebook defines a custom `PlaylistDataset` class to handle data loading and padding for a `GPT2LMHeadModel`. It then trains the model and evaluates its performance using a custom metric.

#### Paths and Parameters
* Change the paths according to your folders and files:
* **Input Path**: `MyDrive/DL_project/csv/tracks_with_ids.parquet`
* **Important Parameters**:
    * **Minimum Playlist Length**: **`30`** (used for filtering)
    * **Data Split Ratio**: **`80/10/10`** (Train/Validation/Test)
    * **Fast Prototyping Subset**: **`25%`** of the data is used for a quick run.
    * **Model**: **`GPT2LMHeadModel`**
    * **Evaluation Metric**: **`recall@20`**
 

