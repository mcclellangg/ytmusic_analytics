# YouTube Music Analytics

YouTube Music Analytics is a Dockerized project designed to extract, process, and analyze YouTube Music listening history. Using data obtained from Google Takeout, it converts your listening history into a structured format for exploration and analytics. This project aims to empower users with deeper insights into their music habits, trends, and listening patterns over time.

## Prerequisites
- Docker
- Docker Compose

## Project Setup and Usage
1. Clone this repository:

 ```bash
   git clone https://github.com/mcclellangg/ytmusic_analytics.git
   cd ytmusic_analytics
   ```

2. Add your YouTube Music data for 2025 into `data/` within the root dir of project (See [Step 5](#update-data))

3. Build and start container to extract data from `watch-history.json` to csv

```bash
docker compose run --rm ytmusic_analytics create-ytm-hist-2025.ipynb
```

4. Remove cell outputs, and metadata from notebooks:

```bash
docker compose run --rm clean_notebooks
```

Note: This command may not be all encompassing and Git may still track other metadata related to the files.

### Getting YouTube Music data from Google Takeout
1. Navigate to [Google Takeout](https://takeout.google.com/)
2. Create a new export
    1. Select YouTube and YouTube Music (ensure all other options are deselected)
        ![Google Takeout - select datasets](img_assets/select_data.png)
    2. Click `Multiple formats` and change `history` to `JSON`, click `OK`
        ![Google Takeout - select data format](img_assets/change_format_to_json.png)
3. Click `Next Step`, select file delivery options then click `Create export`
4. Once export is created, download zip file and extract
#### Update `data/`
5. Locate `watch-history.json` and add it to the `data/` within this project
    1. `watch-history.json` can be found here in extracted file: `Takeout-20250312T231919Z-001.zip\Takeout\YouTube and YouTube Music\history`
    2. NOTE: `watch-history.json` contains YOUR personal data, this project is designed to ignore files with that name, but git will track it should you include that file then change the name
    3. Additionally any files within the `data/` directory other than the included sample file `customers-10000.csv` are designed to be ignored

