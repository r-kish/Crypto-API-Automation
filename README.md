# Cryptocurrency API Automation

The purpose of this project was to showcase the web scraping, and automation capabilities of Python. The value of cryptocurrency is constantly changing by the second, which makes the data associated with it an ideal subject to be analyzed using automated extraction.

Data is extracted from the publicly accessible CoinMarketCap API. Data was captured in real-time, once per minute, over the course of 10 minutes. Code may be updated to accommodate any time interval and duration.

### The following code is used to conduct the automated data extraction from the Cryptocurrency API:
```
def api_runner():
    global df
    url = 'https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest' 
    #Original Sandbox Environment: 'https://sandbox-api.coinmarketcap.com/v1/cryptocurrency/listings/latest'
    parameters = {
      'start':'1',
      'limit':'20',
      'convert':'USD'
    }
    headers = {
      'Accepts': 'application/json',
      'X-CMC_PRO_API_KEY': '0ad53085-1cb2-4eb8-ad9e-3ffbd7e56509',
    }
    session = Session()
    session.headers.update(headers)
    try:
      response = session.get(url, params=parameters)
      data = json.loads(response.text)
      #print(data)
    except (ConnectionError, Timeout, TooManyRedirects) as e:
      print(e)
    df2 = pd.json_normalize(data['data'])
    df2['timestamp'] = pd.to_datetime('now')
    df = df.append(df2)
```
- When run, the CoinMarketCap API is accessed, and data is continuously loaded into a dataframe with each extraction. A for loop is used for determining the duration and frequency of the automatic extraction, and is required for conducting a time series analysis.

## Time series of the percent-change in value of the top 20 cryptocurrencies based on time
- The data for this visualization consists of the average values from all extractions over the duration of the automation. In this case, all 10 instances for each cryptocurrency from the 10-minute data extraction are averaged into one average instance for each cryptocurrency. The percent change in value in USD is provided for each cryptocurrency at the following checkpoints: 1 hour, 24 hour, 7 days, 30 days, 60 days, 90 days.
  
![FullSeries](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_FullSeriesChange.png)

## Change in value in USD of a cryptocurrency over time (Bitcoin and Ethereum)
- The data for these next two visualizations consists of the value in USD for the selected cryptocurrency at each extraction over the duration of the automation's run. In this case, there are 10 different values for each of the 10 extractions conducted in this automation. Notice how much each cryptocurrency fluctuates in just a matter of minutes.
  
### Bitcoin
![Change1](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_BitcoinChange.png)

### Ethereum
![Change2](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_EthereumChange.png)

## Count of Market Pairs for top 7 cryptocurrencies
- Market Pairs refer to a cryptocurrency that can be exchanged for another. The more market pairs available, the more versatile the cryptocurrency.

![MarketPairs](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_MarketPairs.png)
