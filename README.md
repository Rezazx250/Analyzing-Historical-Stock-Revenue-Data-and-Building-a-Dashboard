# Analyzing-Historical-Stock-Revenue-Data-and-Building-a-Dashboard
Analyzing Historical Stock/Revenue Data and Building a Dashboard
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<center>\n",
    "    <img src=\"https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/Images/SN_logo.png\" width=\"300\" alt=\"cognitiveclass.ai logo\">\n",
    "</center>\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h1>Extracting Stock Data Using a Python Library</h1>\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "A company's stock share is a piece of the company more precisely:\n",
    "<p><b>A stock (also known as equity) is a security that represents the ownership of a fraction of a corporation. This\n",
    "entitles the owner of the stock to a proportion of the corporation's assets and profits equal to how much stock they own. Units of stock are called \"shares.\" [1]</p></b>\n",
    "\n",
    "An investor can buy a stock and sell it later. If the stock price increases, the investor profits, If it decreases,the investor with incur a loss.  Determining the stock price is complex; it depends on the number of outstanding shares, the size of the company's future profits, and much more. People trade stocks throughout the day the stock ticker is a report of the price of a certain stock, updated continuously throughout the trading session by the various stock market exchanges. \n",
    "<p>You are a data scientist working for a hedge fund; it's your job to determine any suspicious stock activity. In this lab you will extract stock data using a Python library. We will use the <coode>yfinance</code> library, it allows us to extract data for stocks returning data in a pandas dataframe. You will use the lab to extract.</p>\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2>Table of Contents</h2>\n",
    "<div class=\"alert alert-block alert-info\" style=\"margin-top: 20px\">\n",
    "    <ul>\n",
    "        <li>Using yfinance to Extract Stock Info</li>\n",
    "        <li>Using yfinance to Extract Historical Share Price Data</li>\n",
    "        <li>Using yfinance to Extract Historical Dividends Data</li>\n",
    "        <li>Exercise</li>\n",
    "    </ul>\n",
    "<p>\n",
    "    Estimated Time Needed: <strong>30 min</strong></p>\n",
    "</div>\n",
    "\n",
    "<hr>\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Collecting yfinance==0.2.4\n",
      "  Downloading yfinance-0.2.4-py2.py3-none-any.whl (51 kB)\n",
      "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m51.4/51.4 kB\u001b[0m \u001b[31m8.6 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
      "\u001b[?25hRequirement already satisfied: pandas>=1.3.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (1.3.5)\n",
      "Requirement already satisfied: numpy>=1.16.5 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (1.21.6)\n",
      "Requirement already satisfied: requests>=2.26 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (2.29.0)\n",
      "Collecting multitasking>=0.0.7 (from yfinance==0.2.4)\n",
      "  Downloading multitasking-0.0.11-py3-none-any.whl (8.5 kB)\n",
      "Requirement already satisfied: lxml>=4.9.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (4.9.2)\n",
      "Collecting appdirs>=1.4.4 (from yfinance==0.2.4)\n",
      "  Downloading appdirs-1.4.4-py2.py3-none-any.whl (9.6 kB)\n",
      "Requirement already satisfied: pytz>=2022.5 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (2023.3)\n",
      "Collecting frozendict>=2.3.4 (from yfinance==0.2.4)\n",
      "  Downloading frozendict-2.4.4-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (103 kB)\n",
      "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m103.7/103.7 kB\u001b[0m \u001b[31m18.8 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
      "\u001b[?25hRequirement already satisfied: cryptography>=3.3.2 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (38.0.2)\n",
      "Requirement already satisfied: beautifulsoup4>=4.11.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.2.4) (4.11.1)\n",
      "Collecting html5lib>=1.1 (from yfinance==0.2.4)\n",
      "  Downloading html5lib-1.1-py2.py3-none-any.whl (112 kB)\n",
      "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m112.2/112.2 kB\u001b[0m \u001b[31m21.4 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
      "\u001b[?25hRequirement already satisfied: soupsieve>1.2 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from beautifulsoup4>=4.11.1->yfinance==0.2.4) (2.3.2.post1)\n",
      "Requirement already satisfied: cffi>=1.12 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from cryptography>=3.3.2->yfinance==0.2.4) (1.15.1)\n",
      "Requirement already satisfied: six>=1.9 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from html5lib>=1.1->yfinance==0.2.4) (1.16.0)\n",
      "Requirement already satisfied: webencodings in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from html5lib>=1.1->yfinance==0.2.4) (0.5.1)\n",
      "Requirement already satisfied: python-dateutil>=2.7.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from pandas>=1.3.0->yfinance==0.2.4) (2.8.2)\n",
      "Requirement already satisfied: charset-normalizer<4,>=2 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.26->yfinance==0.2.4) (3.1.0)\n",
      "Requirement already satisfied: idna<4,>=2.5 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.26->yfinance==0.2.4) (3.4)\n",
      "Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.26->yfinance==0.2.4) (1.26.15)\n",
      "Requirement already satisfied: certifi>=2017.4.17 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.26->yfinance==0.2.4) (2023.5.7)\n",
      "Requirement already satisfied: pycparser in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from cffi>=1.12->cryptography>=3.3.2->yfinance==0.2.4) (2.21)\n",
      "Installing collected packages: multitasking, appdirs, html5lib, frozendict, yfinance\n",
      "Successfully installed appdirs-1.4.4 frozendict-2.4.4 html5lib-1.1 multitasking-0.0.11 yfinance-0.2.4\n"
     ]
    }
   ],
   "source": [
    "!pip install yfinance==0.2.4\n",
    "#!pip install pandas==1.3.3"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "import yfinance as yf\n",
    "import pandas as pd"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Using the yfinance Library to Extract Stock Data\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Using the `Ticker` module we can create an object that will allow us to access functions to extract data. To do this we need to provide the ticker symbol for the stock, here the company is Apple and the ticker symbol is `AAPL`.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "apple = yf.Ticker(\"AAPL\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now we can access functions and variables to extract the type of data we need. You can view them and what they represent here https://aroussi.com/post/python-yahoo-finance.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "--2024-06-06 00:47:54--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/data/apple.json\n",
      "Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104, 169.63.118.104\n",
      "Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.\n",
      "HTTP request sent, awaiting response... 200 OK\n",
      "Length: 5699 (5.6K) [application/json]\n",
      "Saving to: ‘apple.json’\n",
      "\n",
      "apple.json          100%[===================>]   5.57K  --.-KB/s    in 0s      \n",
      "\n",
      "2024-06-06 00:47:54 (75.5 MB/s) - ‘apple.json’ saved [5699/5699]\n",
      "\n"
     ]
    }
   ],
   "source": [
    "!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/data/apple.json"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Stock Info\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Using the attribute  <code>info</code> we can extract information about the stock as a Python dictionary.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'zip': '95014',\n",
       " 'sector': 'Technology',\n",
       " 'fullTimeEmployees': 100000,\n",
       " 'longBusinessSummary': 'Apple Inc. designs, manufactures, and markets smartphones, personal computers, tablets, wearables, and accessories worldwide. It also sells various related services. In addition, the company offers iPhone, a line of smartphones; Mac, a line of personal computers; iPad, a line of multi-purpose tablets; AirPods Max, an over-ear wireless headphone; and wearables, home, and accessories comprising AirPods, Apple TV, Apple Watch, Beats products, HomePod, and iPod touch. Further, it provides AppleCare support services; cloud services store services; and operates various platforms, including the App Store that allow customers to discover and download applications and digital content, such as books, music, video, games, and podcasts. Additionally, the company offers various services, such as Apple Arcade, a game subscription service; Apple Music, which offers users a curated listening experience with on-demand radio stations; Apple News+, a subscription news and magazine service; Apple TV+, which offers exclusive original content; Apple Card, a co-branded credit card; and Apple Pay, a cashless payment service, as well as licenses its intellectual property. The company serves consumers, and small and mid-sized businesses; and the education, enterprise, and government markets. It distributes third-party applications for its products through the App Store. The company also sells its products through its retail and online stores, and direct sales force; and third-party cellular network carriers, wholesalers, retailers, and resellers. Apple Inc. was incorporated in 1977 and is headquartered in Cupertino, California.',\n",
       " 'city': 'Cupertino',\n",
       " 'phone': '408 996 1010',\n",
       " 'state': 'CA',\n",
       " 'country': 'United States',\n",
       " 'companyOfficers': [],\n",
       " 'website': 'https://www.apple.com',\n",
       " 'maxAge': 1,\n",
       " 'address1': 'One Apple Park Way',\n",
       " 'industry': 'Consumer Electronics',\n",
       " 'ebitdaMargins': 0.33890998,\n",
       " 'profitMargins': 0.26579002,\n",
       " 'grossMargins': 0.43019,\n",
       " 'operatingCashflow': 112241000448,\n",
       " 'revenueGrowth': 0.112,\n",
       " 'operatingMargins': 0.309,\n",
       " 'ebitda': 128217997312,\n",
       " 'targetLowPrice': 160,\n",
       " 'recommendationKey': 'buy',\n",
       " 'grossProfits': 152836000000,\n",
       " 'freeCashflow': 80153247744,\n",
       " 'targetMedianPrice': 199.5,\n",
       " 'currentPrice': 177.77,\n",
       " 'earningsGrowth': 0.25,\n",
       " 'currentRatio': 1.038,\n",
       " 'returnOnAssets': 0.19875,\n",
       " 'numberOfAnalystOpinions': 44,\n",
       " 'targetMeanPrice': 193.53,\n",
       " 'debtToEquity': 170.714,\n",
       " 'returnOnEquity': 1.45567,\n",
       " 'targetHighPrice': 215,\n",
       " 'totalCash': 63913000960,\n",
       " 'totalDebt': 122797998080,\n",
       " 'totalRevenue': 378323009536,\n",
       " 'totalCashPerShare': 3.916,\n",
       " 'financialCurrency': 'USD',\n",
       " 'revenuePerShare': 22.838,\n",
       " 'quickRatio': 0.875,\n",
       " 'recommendationMean': 1.8,\n",
       " 'exchange': 'NMS',\n",
       " 'shortName': 'Apple Inc.',\n",
       " 'longName': 'Apple Inc.',\n",
       " 'exchangeTimezoneName': 'America/New_York',\n",
       " 'exchangeTimezoneShortName': 'EDT',\n",
       " 'isEsgPopulated': False,\n",
       " 'gmtOffSetMilliseconds': '-14400000',\n",
       " 'quoteType': 'EQUITY',\n",
       " 'symbol': 'AAPL',\n",
       " 'messageBoardId': 'finmb_24937',\n",
       " 'market': 'us_market',\n",
       " 'annualHoldingsTurnover': None,\n",
       " 'enterpriseToRevenue': 7.824,\n",
       " 'beta3Year': None,\n",
       " 'enterpriseToEbitda': 23.086,\n",
       " '52WeekChange': 0.4549594,\n",
       " 'morningStarRiskRating': None,\n",
       " 'forwardEps': 6.56,\n",
       " 'revenueQuarterlyGrowth': None,\n",
       " 'sharesOutstanding': 16319399936,\n",
       " 'fundInceptionDate': None,\n",
       " 'annualReportExpenseRatio': None,\n",
       " 'totalAssets': None,\n",
       " 'bookValue': 4.402,\n",
       " 'sharesShort': 111286790,\n",
       " 'sharesPercentSharesOut': 0.0068,\n",
       " 'fundFamily': None,\n",
       " 'lastFiscalYearEnd': 1632528000,\n",
       " 'heldPercentInstitutions': 0.59397,\n",
       " 'netIncomeToCommon': 100554997760,\n",
       " 'trailingEps': 6.015,\n",
       " 'lastDividendValue': 0.22,\n",
       " 'SandP52WeekChange': 0.15217662,\n",
       " 'priceToBook': 40.38392,\n",
       " 'heldPercentInsiders': 0.0007,\n",
       " 'nextFiscalYearEnd': 1695600000,\n",
       " 'yield': None,\n",
       " 'mostRecentQuarter': 1640390400,\n",
       " 'shortRatio': 1.21,\n",
       " 'sharesShortPreviousMonthDate': 1644883200,\n",
       " 'floatShares': 16302795170,\n",
       " 'beta': 1.185531,\n",
       " 'enterpriseValue': 2959991898112,\n",
       " 'priceHint': 2,\n",
       " 'threeYearAverageReturn': None,\n",
       " 'lastSplitDate': 1598832000,\n",
       " 'lastSplitFactor': '4:1',\n",
       " 'legalType': None,\n",
       " 'lastDividendDate': 1643932800,\n",
       " 'morningStarOverallRating': None,\n",
       " 'earningsQuarterlyGrowth': 0.204,\n",
       " 'priceToSalesTrailing12Months': 7.668314,\n",
       " 'dateShortInterest': 1647302400,\n",
       " 'pegRatio': 1.94,\n",
       " 'ytdReturn': None,\n",
       " 'forwardPE': 27.099087,\n",
       " 'lastCapGain': None,\n",
       " 'shortPercentOfFloat': 0.0068,\n",
       " 'sharesShortPriorMonth': 108944701,\n",
       " 'impliedSharesOutstanding': 0,\n",
       " 'category': None,\n",
       " 'fiveYearAverageReturn': None,\n",
       " 'previousClose': 178.96,\n",
       " 'regularMarketOpen': 178.55,\n",
       " 'twoHundredDayAverage': 156.03505,\n",
       " 'trailingAnnualDividendYield': 0.004833482,\n",
       " 'payoutRatio': 0.1434,\n",
       " 'volume24Hr': None,\n",
       " 'regularMarketDayHigh': 179.61,\n",
       " 'navPrice': None,\n",
       " 'averageDailyVolume10Day': 93823630,\n",
       " 'regularMarketPreviousClose': 178.96,\n",
       " 'fiftyDayAverage': 166.498,\n",
       " 'trailingAnnualDividendRate': 0.865,\n",
       " 'open': 178.55,\n",
       " 'toCurrency': None,\n",
       " 'averageVolume10days': 93823630,\n",
       " 'expireDate': None,\n",
       " 'algorithm': None,\n",
       " 'dividendRate': 0.88,\n",
       " 'exDividendDate': 1643932800,\n",
       " 'circulatingSupply': None,\n",
       " 'startDate': None,\n",
       " 'regularMarketDayLow': 176.7,\n",
       " 'currency': 'USD',\n",
       " 'trailingPE': 29.55445,\n",
       " 'regularMarketVolume': 92633154,\n",
       " 'lastMarket': None,\n",
       " 'maxSupply': None,\n",
       " 'openInterest': None,\n",
       " 'marketCap': 2901099675648,\n",
       " 'volumeAllCurrencies': None,\n",
       " 'strikePrice': None,\n",
       " 'averageVolume': 95342043,\n",
       " 'dayLow': 176.7,\n",
       " 'ask': 178.53,\n",
       " 'askSize': 800,\n",
       " 'volume': 92633154,\n",
       " 'fiftyTwoWeekHigh': 182.94,\n",
       " 'fromCurrency': None,\n",
       " 'fiveYearAvgDividendYield': 1.13,\n",
       " 'fiftyTwoWeekLow': 122.25,\n",
       " 'bid': 178.4,\n",
       " 'tradeable': False,\n",
       " 'dividendYield': 0.005,\n",
       " 'bidSize': 3200,\n",
       " 'dayHigh': 179.61,\n",
       " 'regularMarketPrice': 177.77,\n",
       " 'preMarketPrice': 178.38,\n",
       " 'logo_url': 'https://logo.clearbit.com/apple.com'}"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import json\n",
    "with open('apple.json') as json_file:\n",
    "    apple_info = json.load(json_file)\n",
    "    # Print the type of data variable    \n",
    "    #print(\"Type:\", type(apple_info))\n",
    "apple_info"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can get the <code>'country'</code> using the key country\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'United States'"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "apple_info['country']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Extracting Share Price\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "A share is the single smallest part of a company's stock  that you can buy, the prices of these shares fluctuate over time. Using the <code>history()</code> method we can get the share price of the stock over a certain period of time. Using the `period` parameter we can set how far back from the present to get data. The options for `period` are 1 day (1d), 5d, 1 month (1mo) , 3mo, 6mo, 1 year (1y), 2y, 5y, 10y, ytd, and max.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "apple_share_price_data = apple.history(period=\"max\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The format that the data is returned in is a Pandas DataFrame. With the `Date` as the index the share `Open`, `High`, `Low`, `Close`, `Volume`, and `Stock Splits` are given for each day.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Open</th>\n",
       "      <th>High</th>\n",
       "      <th>Low</th>\n",
       "      <th>Close</th>\n",
       "      <th>Volume</th>\n",
       "      <th>Dividends</th>\n",
       "      <th>Stock Splits</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1980-12-12 00:00:00-05:00</th>\n",
       "      <td>0.099058</td>\n",
       "      <td>0.099488</td>\n",
       "      <td>0.099058</td>\n",
       "      <td>0.099058</td>\n",
       "      <td>469033600</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1980-12-15 00:00:00-05:00</th>\n",
       "      <td>0.094321</td>\n",
       "      <td>0.094321</td>\n",
       "      <td>0.093890</td>\n",
       "      <td>0.093890</td>\n",
       "      <td>175884800</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1980-12-16 00:00:00-05:00</th>\n",
       "      <td>0.087429</td>\n",
       "      <td>0.087429</td>\n",
       "      <td>0.086998</td>\n",
       "      <td>0.086998</td>\n",
       "      <td>105728000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1980-12-17 00:00:00-05:00</th>\n",
       "      <td>0.089152</td>\n",
       "      <td>0.089582</td>\n",
       "      <td>0.089152</td>\n",
       "      <td>0.089152</td>\n",
       "      <td>86441600</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1980-12-18 00:00:00-05:00</th>\n",
       "      <td>0.091737</td>\n",
       "      <td>0.092167</td>\n",
       "      <td>0.091737</td>\n",
       "      <td>0.091737</td>\n",
       "      <td>73449600</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                               Open      High       Low     Close     Volume  \\\n",
       "Date                                                                           \n",
       "1980-12-12 00:00:00-05:00  0.099058  0.099488  0.099058  0.099058  469033600   \n",
       "1980-12-15 00:00:00-05:00  0.094321  0.094321  0.093890  0.093890  175884800   \n",
       "1980-12-16 00:00:00-05:00  0.087429  0.087429  0.086998  0.086998  105728000   \n",
       "1980-12-17 00:00:00-05:00  0.089152  0.089582  0.089152  0.089152   86441600   \n",
       "1980-12-18 00:00:00-05:00  0.091737  0.092167  0.091737  0.091737   73449600   \n",
       "\n",
       "                           Dividends  Stock Splits  \n",
       "Date                                                \n",
       "1980-12-12 00:00:00-05:00        0.0           0.0  \n",
       "1980-12-15 00:00:00-05:00        0.0           0.0  \n",
       "1980-12-16 00:00:00-05:00        0.0           0.0  \n",
       "1980-12-17 00:00:00-05:00        0.0           0.0  \n",
       "1980-12-18 00:00:00-05:00        0.0           0.0  "
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "apple_share_price_data.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can reset the index of the DataFrame with the `reset_index` function. We also set the `inplace` paramter to `True` so the change takes place to the DataFrame itself.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "apple_share_price_data.reset_index(inplace=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can plot the `Open` price against the `Date`:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:xlabel='Date'>"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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3759O0QE8fHxdsvNZjOCg4NdHj8RuR4LFCKqdvv27UNcXByOHz+O2267DU8++SRee+01BAUFYf369Rg+fDjy8vJK3d9ms0Gv12Pbtm3Q6/V26+rUqePq8ImoGrBAIaJqtWrVKuzevRvPPfcctm7divz8fLz33nvQ6Qq6xH3//fd22xuNRlitVrtlbdq0gdVqRVpaGrp161ZtsRNR9WGBQkQuYzabkZqaCqvVirNnz+K3335DUlIS7rjjDjz88MPYvXs38vPz8fHHH2PAgAHYsGEDPvvsM7tjxMbGIisrC7///jtat24NX19fxMfHY+jQoXj44Yfx3nvvoU2bNjh//jxWrVqFli1b4rbbbnNTxkTkLLyLh4hc5rfffkNERARiY2PRv39/rF69Gh999BF++ukn6PV63HTTTZg6dSreeusttGjRAnPmzEFSUpLdMbp06YInn3wS999/P+rXr4+3334bADBjxgw8/PDDGD9+PJo2bYo777wTf/75J6Kjo92RKhE5Ge/iISIiIs1hCwoRERFpDgsUIiIi0hwWKERERKQ5LFCIiIhIc1igEBERkeawQCEiIiLNYYFCREREmsMChYiIiDSHBQoRERFpDgsUIiIi0hwWKERERKQ5/x9IE4jlpmyXKQAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "apple_share_price_data.plot(x=\"Date\", y=\"Open\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Extracting Dividends\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Dividends are the distribution of a companys profits to shareholders. In this case they are defined as an amount of money returned per share an investor owns. Using the variable `dividends` we can get a dataframe of the data. The period of the data is given by the period defined in the 'history` function.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Date\n",
       "1987-05-11 00:00:00-04:00    0.000536\n",
       "1987-08-10 00:00:00-04:00    0.000536\n",
       "1987-11-17 00:00:00-05:00    0.000714\n",
       "1988-02-12 00:00:00-05:00    0.000714\n",
       "1988-05-16 00:00:00-04:00    0.000714\n",
       "                               ...   \n",
       "2023-05-12 00:00:00-04:00    0.240000\n",
       "2023-08-11 00:00:00-04:00    0.240000\n",
       "2023-11-10 00:00:00-05:00    0.240000\n",
       "2024-02-09 00:00:00-05:00    0.240000\n",
       "2024-05-10 00:00:00-04:00    0.250000\n",
       "Name: Dividends, Length: 83, dtype: float64"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "apple.dividends"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can plot the dividends overtime:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:xlabel='Date'>"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "apple.dividends.plot()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Exercise \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now using the `Ticker` module create an object for AMD (Advanced Micro Devices) with the ticker symbol is `AMD` called; name the object <code>amd</code>.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "amd = yf.Ticker(\"amd\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "--2024-06-06 00:56:13--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/data/amd.json\n",
      "Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104, 169.63.118.104\n",
      "Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.\n",
      "HTTP request sent, awaiting response... 200 OK\n",
      "Length: 5838 (5.7K) [application/json]\n",
      "Saving to: ‘amd.json.1’\n",
      "\n",
      "amd.json.1          100%[===================>]   5.70K  --.-KB/s    in 0s      \n",
      "\n",
      "2024-06-06 00:56:13 (61.4 MB/s) - ‘amd.json.1’ saved [5838/5838]\n",
      "\n"
     ]
    }
   ],
   "source": [
    "!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/data/amd.json"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'zip': '95054',\n",
       " 'sector': 'Technology',\n",
       " 'fullTimeEmployees': 15500,\n",
       " 'longBusinessSummary': 'Advanced Micro Devices, Inc. operates as a semiconductor company worldwide. The company operates in two segments, Computing and Graphics; and Enterprise, Embedded and Semi-Custom. Its products include x86 microprocessors as an accelerated processing unit, chipsets, discrete and integrated graphics processing units (GPUs), data center and professional GPUs, and development services; and server and embedded processors, and semi-custom System-on-Chip (SoC) products, development services, and technology for game consoles. The company provides processors for desktop and notebook personal computers under the AMD Ryzen, AMD Ryzen PRO, Ryzen Threadripper, Ryzen Threadripper PRO, AMD Athlon, AMD Athlon PRO, AMD FX, AMD A-Series, and AMD PRO A-Series processors brands; discrete GPUs for desktop and notebook PCs under the AMD Radeon graphics, AMD Embedded Radeon graphics brands; and professional graphics products under the AMD Radeon Pro and AMD FirePro graphics brands. It also offers Radeon Instinct, Radeon PRO V-series, and AMD Instinct accelerators for servers; chipsets under the AMD trademark; microprocessors for servers under the AMD EPYC; embedded processor solutions under the AMD Athlon, AMD Geode, AMD Ryzen, AMD EPYC, AMD R-Series, and G-Series processors brands; and customer-specific solutions based on AMD CPU, GPU, and multi-media technologies, as well as semi-custom SoC products. It serves original equipment manufacturers, public cloud service providers, original design manufacturers, system integrators, independent distributors, online retailers, and add-in-board manufacturers through its direct sales force, independent distributors, and sales representatives. The company was incorporated in 1969 and is headquartered in Santa Clara, California.',\n",
       " 'city': 'Santa Clara',\n",
       " 'phone': '408 749 4000',\n",
       " 'state': 'CA',\n",
       " 'country': 'United States',\n",
       " 'companyOfficers': [],\n",
       " 'website': 'https://www.amd.com',\n",
       " 'maxAge': 1,\n",
       " 'address1': '2485 Augustine Drive',\n",
       " 'industry': 'Semiconductors',\n",
       " 'ebitdaMargins': 0.24674,\n",
       " 'profitMargins': 0.19240999,\n",
       " 'grossMargins': 0.48248002,\n",
       " 'operatingCashflow': 3520999936,\n",
       " 'revenueGrowth': 0.488,\n",
       " 'operatingMargins': 0.22198,\n",
       " 'ebitda': 4055000064,\n",
       " 'targetLowPrice': 107,\n",
       " 'recommendationKey': 'buy',\n",
       " 'grossProfits': 7929000000,\n",
       " 'freeCashflow': 3122749952,\n",
       " 'targetMedianPrice': 150,\n",
       " 'currentPrice': 119.22,\n",
       " 'earningsGrowth': -0.454,\n",
       " 'currentRatio': 2.024,\n",
       " 'returnOnAssets': 0.21327,\n",
       " 'numberOfAnalystOpinions': 38,\n",
       " 'targetMeanPrice': 152.02,\n",
       " 'debtToEquity': 9.764,\n",
       " 'returnOnEquity': 0.47428,\n",
       " 'targetHighPrice': 200,\n",
       " 'totalCash': 3608000000,\n",
       " 'totalDebt': 732000000,\n",
       " 'totalRevenue': 16433999872,\n",
       " 'totalCashPerShare': 3.008,\n",
       " 'financialCurrency': 'USD',\n",
       " 'revenuePerShare': 13.548,\n",
       " 'quickRatio': 1.49,\n",
       " 'recommendationMean': 2.2,\n",
       " 'exchange': 'NMS',\n",
       " 'shortName': 'Advanced Micro Devices, Inc.',\n",
       " 'longName': 'Advanced Micro Devices, Inc.',\n",
       " 'exchangeTimezoneName': 'America/New_York',\n",
       " 'exchangeTimezoneShortName': 'EDT',\n",
       " 'isEsgPopulated': False,\n",
       " 'gmtOffSetMilliseconds': '-14400000',\n",
       " 'quoteType': 'EQUITY',\n",
       " 'symbol': 'AMD',\n",
       " 'messageBoardId': 'finmb_168864',\n",
       " 'market': 'us_market',\n",
       " 'annualHoldingsTurnover': None,\n",
       " 'enterpriseToRevenue': 8.525,\n",
       " 'beta3Year': None,\n",
       " 'enterpriseToEbitda': 34.551,\n",
       " '52WeekChange': 0.51966953,\n",
       " 'morningStarRiskRating': None,\n",
       " 'forwardEps': 4.72,\n",
       " 'revenueQuarterlyGrowth': None,\n",
       " 'sharesOutstanding': 1627360000,\n",
       " 'fundInceptionDate': None,\n",
       " 'annualReportExpenseRatio': None,\n",
       " 'totalAssets': None,\n",
       " 'bookValue': 6.211,\n",
       " 'sharesShort': 27776129,\n",
       " 'sharesPercentSharesOut': 0.0171,\n",
       " 'fundFamily': None,\n",
       " 'lastFiscalYearEnd': 1640390400,\n",
       " 'heldPercentInstitutions': 0.52896,\n",
       " 'netIncomeToCommon': 3161999872,\n",
       " 'trailingEps': 2.57,\n",
       " 'lastDividendValue': 0.005,\n",
       " 'SandP52WeekChange': 0.15217662,\n",
       " 'priceToBook': 19.194977,\n",
       " 'heldPercentInsiders': 0.00328,\n",
       " 'nextFiscalYearEnd': 1703462400,\n",
       " 'yield': None,\n",
       " 'mostRecentQuarter': 1640390400,\n",
       " 'shortRatio': 0.24,\n",
       " 'sharesShortPreviousMonthDate': 1644883200,\n",
       " 'floatShares': 1193798619,\n",
       " 'beta': 1.848425,\n",
       " 'enterpriseValue': 140104957952,\n",
       " 'priceHint': 2,\n",
       " 'threeYearAverageReturn': None,\n",
       " 'lastSplitDate': 966902400,\n",
       " 'lastSplitFactor': '2:1',\n",
       " 'legalType': None,\n",
       " 'lastDividendDate': 798940800,\n",
       " 'morningStarOverallRating': None,\n",
       " 'earningsQuarterlyGrowth': -0.453,\n",
       " 'priceToSalesTrailing12Months': 11.805638,\n",
       " 'dateShortInterest': 1647302400,\n",
       " 'pegRatio': 0.99,\n",
       " 'ytdReturn': None,\n",
       " 'forwardPE': 25.258476,\n",
       " 'lastCapGain': None,\n",
       " 'shortPercentOfFloat': 0.0171,\n",
       " 'sharesShortPriorMonth': 88709340,\n",
       " 'impliedSharesOutstanding': 0,\n",
       " 'category': None,\n",
       " 'fiveYearAverageReturn': None,\n",
       " 'previousClose': 123.23,\n",
       " 'regularMarketOpen': 123.04,\n",
       " 'twoHundredDayAverage': 116.6998,\n",
       " 'trailingAnnualDividendYield': 0,\n",
       " 'payoutRatio': 0,\n",
       " 'volume24Hr': None,\n",
       " 'regularMarketDayHigh': 125.66,\n",
       " 'navPrice': None,\n",
       " 'averageDailyVolume10Day': 102167370,\n",
       " 'regularMarketPreviousClose': 123.23,\n",
       " 'fiftyDayAverage': 115.95,\n",
       " 'trailingAnnualDividendRate': 0,\n",
       " 'open': 123.04,\n",
       " 'toCurrency': None,\n",
       " 'averageVolume10days': 102167370,\n",
       " 'expireDate': None,\n",
       " 'algorithm': None,\n",
       " 'dividendRate': None,\n",
       " 'exDividendDate': 798940800,\n",
       " 'circulatingSupply': None,\n",
       " 'startDate': None,\n",
       " 'regularMarketDayLow': 118.59,\n",
       " 'currency': 'USD',\n",
       " 'trailingPE': 46.389107,\n",
       " 'regularMarketVolume': 99476946,\n",
       " 'lastMarket': None,\n",
       " 'maxSupply': None,\n",
       " 'openInterest': None,\n",
       " 'marketCap': 194013855744,\n",
       " 'volumeAllCurrencies': None,\n",
       " 'strikePrice': None,\n",
       " 'averageVolume': 102428813,\n",
       " 'dayLow': 118.59,\n",
       " 'ask': 117.24,\n",
       " 'askSize': 1100,\n",
       " 'volume': 99476946,\n",
       " 'fiftyTwoWeekHigh': 164.46,\n",
       " 'fromCurrency': None,\n",
       " 'fiveYearAvgDividendYield': None,\n",
       " 'fiftyTwoWeekLow': 72.5,\n",
       " 'bid': 117.24,\n",
       " 'tradeable': False,\n",
       " 'dividendYield': None,\n",
       " 'bidSize': 900,\n",
       " 'dayHigh': 125.66,\n",
       " 'regularMarketPrice': 119.22,\n",
       " 'preMarketPrice': 116.98,\n",
       " 'logo_url': 'https://logo.clearbit.com/amd.com'}"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import json\n",
    "with open('amd.json') as json_file:\n",
    "    amd_info = json.load(json_file)\n",
    "    # Print the type of data variable    \n",
    "    #print(\"Type:\", type(apple_info))\n",
    "amd_info"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question 1</b> Use the key  <code>'country'</code> to find the country the stock belongs to, remember it as it will be a quiz question.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'United States'"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "amd_info['country']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question 2</b> Use the key  <code>'sector'</code> to find the sector the stock belongs to, remember it as it will be a quiz question.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'Technology'"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "amd_info['sector']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question 3</b> Obtain stock data for AMD using the `history` function, set the `period` to max. Find the `Volume` traded on the first day (first row).\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<bound method NDFrame.head of                                  Open        High         Low       Close  \\\n",
       "Date                                                                        \n",
       "1980-03-17 00:00:00-05:00    0.000000    3.302083    3.125000    3.145833   \n",
       "1980-03-18 00:00:00-05:00    0.000000    3.125000    2.937500    3.031250   \n",
       "1980-03-19 00:00:00-05:00    0.000000    3.083333    3.020833    3.041667   \n",
       "1980-03-20 00:00:00-05:00    0.000000    3.062500    3.010417    3.010417   \n",
       "1980-03-21 00:00:00-05:00    0.000000    3.020833    2.906250    2.916667   \n",
       "...                               ...         ...         ...         ...   \n",
       "2024-05-30 00:00:00-04:00  167.899994  168.750000  163.800003  166.750000   \n",
       "2024-05-31 00:00:00-04:00  166.649994  169.500000  160.070007  166.899994   \n",
       "2024-06-03 00:00:00-04:00  170.820007  171.080002  160.910004  163.550003   \n",
       "2024-06-04 00:00:00-04:00  162.839996  164.830002  158.869995  159.990005   \n",
       "2024-06-05 00:00:00-04:00  162.070007  167.119995  161.380005  166.169998   \n",
       "\n",
       "                             Volume  Dividends  Stock Splits  \n",
       "Date                                                          \n",
       "1980-03-17 00:00:00-05:00    219600        0.0           0.0  \n",
       "1980-03-18 00:00:00-05:00    727200        0.0           0.0  \n",
       "1980-03-19 00:00:00-05:00    295200        0.0           0.0  \n",
       "1980-03-20 00:00:00-05:00    159600        0.0           0.0  \n",
       "1980-03-21 00:00:00-05:00    130800        0.0           0.0  \n",
       "...                             ...        ...           ...  \n",
       "2024-05-30 00:00:00-04:00  46479900        0.0           0.0  \n",
       "2024-05-31 00:00:00-04:00  64331900        0.0           0.0  \n",
       "2024-06-03 00:00:00-04:00  59157600        0.0           0.0  \n",
       "2024-06-04 00:00:00-04:00  48157200        0.0           0.0  \n",
       "2024-06-05 00:00:00-04:00  59621085        0.0           0.0  \n",
       "\n",
       "[11149 rows x 7 columns]>"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "amd_share_price_data = amd.history(period=\"max\")\n",
    "amd_share_price_data.head"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2>About the Authors:</h2> \n",
    "\n",
    "<a href=\"https://www.linkedin.com/in/joseph-s-50398b136/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork900-2022-01-01\">Joseph Santarcangelo</a> has a PhD in Electrical Engineering, his research focused on using machine learning, signal processing, and computer vision to determine how videos impact human cognition. Joseph has been working for IBM since he completed his PhD.\n",
    "\n",
    "Azim Hirjani\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Change Log\n",
    "\n",
    "| Date (YYYY-MM-DD) | Version | Changed By    | Change Description        |\n",
    "| ----------------- | ------- | ------------- | ------------------------- |\n",
    "| 2020-11-10        | 1.1     | Malika Singla | Deleted the Optional part |\n",
    "| 2020-08-27        | 1.0     | Malika Singla | Added lab to GitLab       |\n",
    "\n",
    "<hr>\n",
    "\n",
    "## <h3 align=\"center\"> © IBM Corporation 2020. All rights reserved. <h3/>\n",
    "\n",
    "<p>\n"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python",
   "language": "python",
   "name": "conda-env-python-py"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.12"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
