# Installation of Cuemacro Python libraries (finmarketpy, findatapy and chartpy)

In order to install the Cuemacro Python libraries, I would recommend following the instructions below. If you already have Python 3.5 installed,
you can skip most of these steps. The below instructions assume that you have not installed Python at all and are assuming you have Windows.

At the end I also have comments related to installing finmarketpy, findatapy and chartpy on Linux and OS X.

In general, I would go in the order suggested here. Some components are optional (such as Bloomberg software).

Before installation of any specific Python libraries, we need to set up the core Python distribution and other programming tools,
which will help you to write your own trading strategies and Python scripts for doing market analysis on top of the Cuemacro libraries.

* Programming Tools
    * Anaconda 4.20 64 bit Python - (download)[https://www.continuum.io/downloads] - Windows/Linux/Mac OS X
      * This is the most used Python distribution for data science.
      * As well as installing the core Python libraries, it also installs many useful libraries like the SciPy stack, which
    contains NumPy, pandas etc. and many other useful libraries like matplotlib which are dependencies for the various Cuemacro libraries
    * Microsoft Visual Studio 2015 Community Edition- (download)[https://www.visualstudio.com/downloads/] - Windows
      * Makes sure to do a custom installation and tick Visual C++
      * Some Python libraries need a C++ compiler in order to build (such as blpapi and arctic)
      * Alternatively, if you don't want to compile the libraries yourself, you can sometimes find pre-compiled
      Python wheels for your platform
    * Git - https://git-scm.com/downloads - Windows/Linux/Mac OS X
        * Version control software
        * Makes it easier to maintain your own Python code
    * PyCharm Community Edition - (download)[https://www.jetbrains.com/pycharm/download/] - Windows/Linux/Mac OS X
        * PyCharm is a relatively easy to use IDE for coding Python, with tools such as autocompletion
        and syntax highlighting
        * The Professional Edition also adds a few bells and whistles including a code profiler
        * I recommend running the 64 bit version, as I've the 32 bit version can run out memory particularly when building
        indices
        * There are many other alternative IDEs (and text editors) for Python, I recommend you also check out a few to see which you like most
        (or you just use a simple text editor like Notepad!)
            * Atom - (hackable text editor, which also has addins for Python coding)[https://atom.io/]
            * PyDev - (Python IDE written on top of Eclipse)[http://www.pydev.org/]
            * Sublime - (text editor)[http://www.sublimetext.com/3]

* Data tools
    * MongoDB - (download)[https://www.mongodb.com/download-center] - Windows/Linux/Mac OS X
        * IOEngine class has a wrapper over arctic to use MongoDB (as well as HDF5 files if preferred)
        * Be sure to setup a database in MongoDB, if you wish to use it to store data later
    * Bloomberg Terminal - (download)[https://www.bloomberg.com/professional/downloads/] - Windows
        * Bloomberg provides a high quality data source
        * Note, you must be a subscriber for the software to function and to have a licence for the data
        * This also needs installation of your serial number
        * Bloomberg Terminal is only available for Windows
        * Bloomberg Server API is available for Windows/Linux/Mac OS X but runs under a different licence

* Fonts
    * Open Sans - chartpy uses the open source Open Sans family of fonts as default for matplotlib
    * You can download these for free from [Font Squirrel](https://www.fontsquirrel.com/fonts/open-sans)
    * Once downloaded you need to install them on your operating system
    * On Windows this involves copying the fonts to your Windows font folder (eg. C:\Windows\Fonts)

Open up the Anaconda Command Prompt (accessible via the Start Menu) to run the various "pip" commands to install the
various Python libraries. The Cuemacro libraries will install most Python dependencies, but some need to be installed separately.

* Python libraries (open source)
    * arctic - pip install git+https://github.com/manahl/arctic.git
        * Wrapper for MongoDB
        * Allows us to easily save and load pandas DataFrames in MongoDB
        * Also compresses data contents
    * blpapi - https://www.bloomberglabs.com/api/libraries/ (both C++ and Python libraries)
        * Interact with Bloomberg programmatically via Python to download historical and live data
        * Note that this requires a C++ compiler to build the Python library (at present Bloomberg doesn't have binaries for Python 3.5,
        hence you need to build them yourself)
        * Follow instructions at https://www.github.com/cuemacro/findatapy/BLOOMBERG.md for all the steps necessary to install blpapi
    * Whilst Anaconda has most of the dependencies below and pip will install all additional ones needed by the Cuemacro Python
    libraries it is possible to install them manually via pip, below is a list of the dependencies
        * all libraries
            * numpy - matrix algebra
            * pandas - time series
            * pytz - timezone management
            * requests - accessing URLs
            * mulitprocessing_on_dill - multitasking
        * findatapy
            * pandas_datareader - accessing market data sources including Yahoo Finance
            * quandl - accessing market data sources
            * openpyxl - writing Excel spreadsheets to disk
        * chartpy
            * bokeh - visualisation
            * cufflinks - wrapper on plotly
            * matplotlib - visualisation
            * plotly - visualisation

* Cuemacro Python libraries (open source)
    * Before we start, make sure we are familiar where your Anaconda site packages folder is (ie. where it will install your Python dependencies),
    as this will be where we need to edit the various configuation files described in this section.
        * Typically this is in folders like:
            * C:\Anaconda3-64\Lib\site-packages
            * C:\Program Files\Anaconda\Lib\site-packages
    * chartpy - pip install git+https://github.com/cuemacro/chartpy.git
        * Check constants file configuration [chartpy/chartpy/util/chartconstants.py](https://github.com/cuemacro/finmarketpy/blob/master/chartpy/util/chartconstants.py) for
            * Adding your own API keys for Plotly, Twitter etc
            * Changing the default size of plots
            * Changing the default chart engine (eg. Plotly, Bokeh or Matplotlib)
        * Alternatively you can create chartcred.py class in the same folder to put your own API keys
        * This has the benefit of not being overwritten each time you upgrade the project
    * findatapy - pip install git+https://github.com/cuemacro/findatapy.git
        * Check constants file configuration [findatapy/findatapy/util/dataconstants.py](https://github.com/cuemacro/finmarketpy/blob/master/findatatpy/util/dataconstants.py) for
            * adding your own API keys for Quandl etc.
            * changing path of your local data source (change `folder_time_series_data` attribute)
            * changing path of the config folder if necessary (change `config_root_folder` attribute) so you can create your own custom
            ticker library (otherwise it will get overwritten each time you upgrade findatapy)
            * changing the ticker library (placed in findatapy/findatapy/conf/*.CSV files)
                * time_series_categories_fields.csv - define the categories of tickers and what fields they have
                * time_series_fields_list.csv - define aliases for vendor fields
                * time_series_tickers_list.csv - defines aliases for vendor tickers
                * There are already setup with a few FX tickers to act as an example
                * In addition, you can add your own CSV files to accompany time_series_tickers_list.csv (add to time_series_tickers_list attribute)
                    * fx_vol_tickers.csv - samples of FX vol tickers
                    * fx_forward_tickers.csv - samples of FX forward tickers
            * changing logging.conf
                * For customising how the project dumps logs to disk
        * Alternatively you can create datacred.py class in the same folder to put your own API keys and file folder settings.
        * This has the benefit of not being overwritten each time you upgrade the project.
        * Below we have a sample:

        ```python
class DataCred(object):

    folder_historic_CSV = "E:/tickdata/historicCSV"
    folder_time_series_data = "C:/timeseriesdata"

    config_root_folder = "E:/Remote/canary/"

    ###### FOR ALIAS TICKERS
    # config file for time series categories
    time_series_categories_fields = \
        config_root_folder + "conf/time_series_categories_fields.csv"

    # we can have multiple tickers files (separated by ";")
    time_series_tickers_list = config_root_folder + "conf/time_series_tickers_list.csv;" + \
                               config_root_folder + "conf/futures_contracts_tickers.csv"


    time_series_fields_list = config_root_folder + "conf/time_series_fields_list.csv"

    # config file for long term econ data
    all_econ_tickers = config_root_folder + "conf/all_econ_tickers.csv"
    econ_country_codes = config_root_folder + "conf/econ_country_codes.csv"
    econ_country_groups = config_root_folder + "conf/econ_country_groups.csv"

    default_market_data_generator = "marketdatagenerator"

    # Quandl settings
    quandl_api_key = "XYZ"

    # Twitter settings (you need to set these up on Twitter)
    TWITTER_APP_KEY = "XYZ"
    TWITTER_APP_SECRET = "XYZ"
    TWITTER_OAUTH_TOKEN = "XYZ"
    TWITTER_OAUTH_TOKEN_SECRET = "XYZ"

    # FRED API key
    fred_api_key = "XYZ"

        ```
    * finmarketpy - pip install git+https://github.com/cuemacro/finmarketpy.git
        * Check constants file configuration [finmarketpy/finmarketpy/util/marketconstants.py](https://github.com/cuemacro/finmarketpy/blob/master/finmarketpy/util/marketconstants.py)
        * You can also create your own file marketcred.py (placed in the same folder)

You can then setup your own separate project in PyCharm to create your own trading strategies on top of it. I'd recommend creating
a private (or public) project on GitHub so you can use it for version control and backup on the cloud. BitBucket is an alternative online
site for hosting a Git server. It also possible to setup a Git server locally as well.

# Installing on Linux and Mac OS X

I have been able to install both chartpy and finmarketpy both Linux and Mac OS X, using the Anaconda distribution
for both platforms.

However, with findatapy, I would note that following:
* Bloomberg Terminal software is only available for windows, so Bloomberg Desktop API (DAPI) is not available for either Linux or Mac OS X,
blpapi needs the Bloomberg Terminal software to communicate with
* However, it should be possible to install Bloomberg Server API (SAPI) which allows blpapi to communicate with it, but you need
a different Bloomberg server licence to run that
* To install arctic on Mac OS X, read [this useful thread](https://github.com/manahl/arctic/issues/14), which gives suggestions
on ways of solving issues when installing arctic on Mac OS X, in particular around the C compiler

* Even if you cannot install blpapi and arctic, findatapy will still install and largely function, but you won't be able
to make Bloomberg calls, or calls via arctic to MongoDB
