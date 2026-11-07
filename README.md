# Restaurant Recommender — Batch Importer

A Python importer that pulls Toronto restaurant data from the Yelp API and stores it in PostgreSQL for the [API server](https://github.com/thomas-m1/restaurant_recommender_server) and [React client](https://github.com/thomas-m1/restaurant_recommender_client).

## Features

- Searches restaurants by occasion and dining preferences
- Skips closed restaurants and results with low ratings or few reviews
- Calculates distance from Toronto City Hall
- Enriches results with available dining attributes and business hours
- Updates existing records and logs each batch run

## Local development

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Create a `.env` file:

```dotenv
YELP_API_KEY=your_yelp_api_key
DATABASE_URL=postgresql://restaurant_user:restaurant_password@localhost:5432/restaurant_db
```

Start PostgreSQL, then run:

```sh
python batch_ingest.py
```

The importer creates the `businesses` table when needed. The server's Alembic migrations create the recommendations table afterward. Keep credentials and batch logs out of version control.

## Configuration

Search terms are defined in `SCENARIO_TERMS`. The pagination loop controls the number of results fetched per term in increments of 50, subject to Yelp API limits.

The default reference coordinates are `DEFAULT_LAT` and `DEFAULT_LON`, set to Toronto City Hall. Reingest existing records when changing the distance origin. The database field `distance_from_office_km` retains its original name for compatibility with the API and client.
