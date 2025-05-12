# YouTube Music Analytics

YouTube Music Analytics is a Dockerized project designed to extract, process, and analyze YouTube Music listening history. Using data obtained from Google Takeout, it converts your listening history into a structured format for exploration and analytics. This project aims to empower users with deeper insights into their music habits, trends, and listening patterns over time.

## Table of Contents
- [Project Setup](#project-setup)
  - [Getting YouTube Music Data](#getting-youtube-music-data-from-google-takeout)
  - [Docker Configuration](#docker-configuration)
  - [Data Extraction](#data-extraction)

## Project Setup

### Getting YouTube Music data from Google Takeout
1. Navigate to [Google Takeout](https://takeout.google.com/)
2. Create a new export
    1. Select YouTube and YouTube Music (ensure all other options are deselected)
        ![Google Takeout - select datasets](img_assets/select_data.png)
    2. Click `Multiple formats` and change `history` to `JSON`, click `OK`
        ![Google Takeout - select data format](img_assets/change_format_to_json.png)
3. Click `Next Step`, select file delivery options then click `Create export`
4. Once export is created, download zip file and extract
5. Locate `watch-history.json` and add it to the `/data` within this project
    1. `watch-history.json` can be found here in extracted file: `Takeout-20250312T231919Z-001.zip\Takeout\YouTube and YouTube Music\history`
    2. NOTE: `watch-history.json` contains YOUR personal data, this project is designed to ignore files with that name, but git will track it should you include that file then change the name
    3. Additionally any files within the `data/` directory other than the included sample file `customers-10000.csv` are designed to be ignored


### Docker Configuration
#### Building the Docker Image
To build the Docker image for the project, run:

```bash
docker build -t ytmusic_analytics .
```

#### Running the Container
Below will mount your local project directory to the container and start a Bash shell inside it:

```bash
docker run --rm -it \
-v $(pwd):/workspaces/ytmusic_analytics \
-w /workspaces/ytmusic_analytics \
ytmusic_analytics /bin/bash
```

### Data Extraction
Extract your YouTube Music data (for 2025) from `watch-history.json` into a csv file.

1. Ensure docker image has been built
2. Run `create-ytm-hist-2025.nbconvert.ipynb` manually or via one of the below docker commands:
    
    
    ```bash
    # Run existing notebook in place
    docker run --rm \
    -v $(pwd):/workspaces/ytmusic_analytics \
    -w /workspaces/ytmusic_analytics/notebooks \
    ytmusic_analytics \
    jupyter nbconvert --to notebook --execute --inplace create-ytm-hist-2025.ipynb
    ```


    ```bash
    # Run notebook and save output in a new file named create-ytm-hist-2025.nbconvert.ipynb
    docker run --rm \
    -v $(pwd):/workspaces/ytmusic_analytics \
    -w /workspaces/ytmusic_analytics/notebooks \
    ytmusic_analytics \
    jupyter nbconvert --to notebook --execute create-ytm-hist-2025.ipynb
    ```
    
    Both options will create a csv file `ytm_hist_2025.csv` and save it in `/workspaces/ytmusic_analytics/data` with the following columns:
    `id,song_title,song_artist,listened_ts,youtube_url`
3. Execute below command to clean up notebook output (if you want to exclude outputs from git tracking):
    
    ```bash
    docker run --rm -v $(pwd):/workspaces/ytmusic_analytics -w /workspaces/ytmusic_analytics/notebooks ytmusic_analytics jupyter nbconvert --ClearOutputPreprocessor.enabled=True --clear-output *.ipynb
    ```