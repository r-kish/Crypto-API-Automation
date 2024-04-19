# Crypto API Automation


### The following code is used to conduct the automated pull from the Cryptocurrency API:
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
### Time series of the performance of the top 20 cryptocurrencies
![FullSeries](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_FullSeriesChange.png)

### Change in value of a cryptocurrency over time (Bitcoin and Ethereum)
#### Bitcoin
![Change1](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_BitcoinChange.png)

#### Ethereum
![Change2](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_EthereumChange.png)

### Count of Market Pairs for top 7 cryptocurrencies
![MarketPairs](https://github.com/r-kish/Crypto-API-Automation/blob/main/images/Crypto_MarketPairs.png)
