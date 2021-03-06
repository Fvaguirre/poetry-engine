# Poetry Recommendation Engine

## Set-up

poetry-engine runs on python 3. Get with it!

### VirtualEnv

Install all required packages in a virtual environment (a python 3 virtual env!):

`pip install -r requirements.txt`

### Tests

Run the tests using py.test within the the virtual environment. (The last tags ensures we don't run the virtualenv tests, which are slow, not ours, and don't all pass):

`python -m py.test --ignore=venv`

To run a single file of tests add the test filename at the end:

`python -m py.test --ignore=venv features/tests/test_vocabulary_features.py`

### Update Poems

Scraped poems are entered into a Postgres database in a table called `poetry`. Settings for the database must be set in a .env file.

Poems are scraped from poetryfoundation.org. Poem pages have urls like this:

`https://www.poetryfoundation.org/poems-and-poets/poems/detail/48761`

Not all poem pages have poems. Some poem pages have poems as images, not text, which is currently not supported. 

By default this will overwrite all entries in the database! Future functionality might do better with this, because sometimes poems are taken down!

`python update_poems.py`

### Extract Features

Run this to extract features for all poems in the database. You can choose to overwrite all features or only generate features that don't yet exist in the database.

When adding new features, two things must be updated:

* Add the column to the database! I do this in Postico manually.
* Update the Poetry model in `db_mgmt/db_mgmt.py`!

`python extract_features.py`

### Find Nearest Neighbors

Find the 'nearest neighbor' for each poems by calculating the distance between all other poems and selecting one with the lowest distance. Distance is simply the sum of the differences squared for all features. (Alternatively you can select a subset of features to use.) Features are first normalized before the difference is calculated such that all features give the same weighting.

The script will store a pickle of the results dictionary to `temp/nearest_neighbor.p` such that the results do not need to be re-run every time. Include the `-r` flag if you want to force a re-run.

`python nearest_neighbor.py`

## Broken links

http://www.poetry-engine.com/poem/4793