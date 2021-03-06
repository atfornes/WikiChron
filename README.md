# WikiChron
Data visualization tool for wikis.

It analyzes the history dump of a wiki and give you nice graphs plotting that data.

# Development

## Install
### Dependencies
* Python 3.5.2 or later
* pip3
* [Dash framework](https://plot.ly/dash)
* [Grasia Dash Components](https://github.com/Grasia/grasia-dash-components)
* [pandas](pandas.pydata.org)
* (Production only) [Redis Cache](https://redis.io/)

### Install instructions
Simply run: `pip3 install -r requirements.txt`

### Using a virtual environment
A good pratice is to use a virtual environment in order to isolate the development environment from your personal stuff. This skips issues about having different Python versions, pip packages in the wrong place or requiring sudo privileges and so on.

In order to do this, first install [virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/), either from your package manager or from pip.

Then, create a new virtual environment in the repo root folder with:
`virtualenv -p python3 venv/`

Activate the virtual environment:
`source venv/bin/activate`

And finally, install dependencies here:
`source venv/bin/activate`

## Get a wiki history dump
First, download a xml file with the full history of the wikis you want to analyze (you can use [this nice script](https://github.com/Akronix/wikia_dump_downloader) to do so).

Second, you'll have to process that xml dump using the script: `dump_parser.py` located in the scripts directory.
In order to do this, place your xml file in the data/ directory and run the following command:

`python3 dump_parser.py data/<name_of_your.xml>`

It will create a new csv file with the same name of your xml in the data/ dir folder.

dump_parser.py also support several xml files at once. For instance, you might want to process all xml files in the data folder:

`python3 dump_parser.py data/*.xml`

## Provide some metadata of the wiki
Wikichron needs one thing else in order to visualize your wiki for you.

You need to have a `wikis.json` file in your data_dir/ directory with some metadata of the wikis you want to explore in wikichron.
That file must have an entry for each wiki with, at least, the following fields: name, url, data, pages number and, optionally, a list of user ids from which you want to remove their activity.
There is an example of `wikis.json` file in the data/ directory of this repo for the given sample set of wikis.

Note that the required information in this file will change in the future. Stay tuned to that file and to the new updates coming.

## Run the application
Use: `python3 app.py`

The webapp will be locally available in http://127.0.0.1:8000/

Optionally, you can specify a directory with the csv data of the wikis you want to analyze with the environment variable: `WIKICHRON_DATA_DIR`.

For instance, suppose that your data is stored in `/var/tmp`, you might launch wikichron using that directory with:

`WIKICHRON_DATA_DIR='/var/tmp' python3 app.py`

It will show all the files ending in .csv as wikis available to analyze and plot.

# Deployment
The easiest way is to follow the Dash instructions: https://plot.ly/dash/deployment

There is a script called `deploy.sh` which launches the app with the latest code in master and provides the appropriate arguments. Check it out and modify to suit your needs.

## Setup cache
If you want to run WikiChron in production, you should setup a RedisDB server and add the corresponding parameters to the cache.py file.

Look at the [FlaskCaching documentation](https://pythonhosted.org/Flask-Caching/#rediscache) for more information about caching.
