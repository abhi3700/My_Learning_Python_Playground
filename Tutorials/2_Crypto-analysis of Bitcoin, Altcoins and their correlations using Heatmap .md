![Python playground.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513764609/p6zh3xatztywv0i6zh8r.png)

Hi guys!!....
### This is in continuation of the tutorial series "Python Playground".

[View Part 1 in Steemit](https://steemit.com/utopian-io/@abhi3700/4elnft-python-playground-1-plot-math-like-a-pro)
[View Part 1 in Utopian](https://utopian.io/utopian-io/@abhi3700/4elnft-python-playground-1-plot-math-like-a-pro)

In this tutorial session, we will analysing the cryptocurrency markets using Python language. There are so many cryptocurrencies around and we need to analyze the markets properly, otherwise we end up losing millions of investment.

The charts we will plot here are as follows:
* **'BTC price of single exchange i.e. Kraken' vs 'Date'** - Here, data is fetched from **Quandl** API.

* **'BTC price of multiple exchanges' vs 'Date'** - Here, data is fetched from **Quandl** API. It provides the BTC price data for many different exchanges.  FYI, there is a support for converting into other fiat currencies.

* **'Altcoin's price' vs 'Date'** - Here, data is fetched from **Poloniex** exchange. So, it provides many Altcoin's price data.

* **A heatmap of top 10 cryptocurrencies with each other** - Here using the **Poloniex** exchange's price data, we will plot a tabular heatmap.

#### Note: All the plots in this tutorial is done using [**Plotly**](https://plot.ly/). Unlike [Matplotlib](https://matplotlib.org/), it's very modern and consists huge dataset which can be fetched using plotly API. Also, it is getting widely used in many tech companies because of its ability to plot complex charts using simple codes.
![22.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514103413/mechxsheajvfclqq3dyx.png)


So, in order to do that we need to install few Python-based tools.

## Installing tools
We will be using [Anaconda](https://anaconda.org/) for accessing the features.
* **Create a separate environment**. It's not necessary but recommended so as to avoid conflicts in case of multiple type of projects running in a PC/ laptop.
```conda create --name cryptocurrency-analysis python=3```
![1.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513756658/djllupfxlvxsjb4pivb2.png)

* Now, **enter into the environment** so as to install the required libraries.
```activate cryptocurrency-analysis```
and then install the tools using 
```conda install numpy pandas nb_conda jupyter plotly quandl```
![2.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513852688/szsfaslwjh2lyrxqjsl8.png)

* **Open the jupyter notebook** in your directory (where project files are to be saved)
Use ```jupyter notebook```
![3.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513853123/xvkh1mjqkafq5r5gdjkj.png)

* Now, when the notebook opens, **choose the correct shell** (where all tools are installed)
![1.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513853235/vnagaxnvxjglammeupuc.png)

After choosing the "Cryptocurrency-analysis" notebook it opens like this..
![4.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513853342/ugdljljgm54bufcdai3g.png)

## Coding
* **Import the libraries** - 
[**Pickle**](https://docs.python.org/2/library/pickle.html) is used for serializing and deserializing the bytes of data into a file.
[**Quandl**](https://docs.quandl.com/) is used for accessing data using languages - Python, R. 
![5.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513932674/ftxfvuvwaffveuongejx.png)

```python
import os
import numpy as np
import pandas as pd
import pickle
import quandl
from datetime import datetime
```

[**Plotly**](https://plot.ly/d3-js-for-python-and-pandas-charts/) is used for plotting the charts.
![6.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1513932834/d827vkmq4iv4pkotggpo.png)
Plotly's offline mode enables to upload the charts into the server (one should have account in plotly) later.
The code is as follows:-
```python
import plotly.offline as py
import plotly.graph_objs as go
import plotly.figure_factory as ff
py.init_notebook_mode(connected=True)
```
* **Now, to download the data, use Quandl** - 
the code snippet of *helper function* is as follows: 
```python
def get_quandl_data(quandl_id):
    #changing the name from 'BCHARTS/KRAKENUSD' to 'BCHARTS-KRAKENUSD'
    cache_path = '{}.pkl'.format(quandl_id).replace('/', '-')
    
    print('Downloading {} from Quandl'.format(quandl_id))
    df = quandl.get(quandl_id, returns='pandas') #downloading the data from quandl_id using Quandl
    
    # Saving the file as .pkl format
    df.to_pickle(cache_path) 
    print('Cached {} at {}'.format(quandl_id, cache_data))
    return df
```

Downloading the data using  the command below:
```btc_usd_price_kraken = get_quandl_data('BCHARTS/KRAKENUSD')```
View the dataframe as follows:
```btc_usd_price_kraken.head()```
![7.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514067291/r2puxn3jsua4fo1755b9.png)

Next plot a simple chart b/w *'BTC price'* and *Date*:
Code snippet: 
```python
#Chart the BTC pricing data
btc_trace = go.Scatter(x = btc_usd_price_kraken.index, y = btc_usd_price_kraken['Weighted Price'])

py.plot([btc_trace])
```
![8.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514068515/l8imxfoxsagtybwkhog7.png)

![9.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514068527/hnazzfv7bneax1w3bbvm.png)

* **Now, pull the data from more than one exchange** - 
```python
exchanges = ['COINBASE', 'BITSTAMP', 'ITBIT']
exchange_data = {}
exchange_data['KRAKEN'] = btc_usd_price_kraken

for exchange in exchanges:
    exchange_code = 'BCHARTS/{}USD'.format(exchange)
    exchange_data[exchange] = get_quandl_data(exchange_code)
```
![10.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514070808/jba6bf8emi5ze2ctwzy0.png)

Visualize the data for each exchanges - 
![11.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514070845/noc0mjkzp0ew2jzstarq.png)

![12.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514070863/lowecyyawjc2yv87yyjg.png)

![13.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514070876/f2fvc5xg9szaerais8zs.png)

* **Merge the Price data of different exchanges into a single dataframe** - 
```python
def merge_dfs_on_column(dataframes, labels, col):
    '''
    dataframes = all the dataframes i.e. [exchange_data['COINBASE'],
                                          exchange_data['BITSTAMP'],
                                          exchange_data['ITBIT'],
                                          exchange_data['KRAKEN']]
    
    labels = all the labels i.e. ['COINBASE', 'BITSTAMP', 'ITBIT', 'KRAKEN']
    ''' 
    series_dict = {}
    for index in range(len(dataframes)):
        series_dict[labels[index]] = dataframes[index][col]
        
    return pd.DataFrame(series_dict)
```

```python
# assign the function's output to a single dataframe
btc_usd_datasets = merge_dfs_on_column(list(exchange_data.values()), list(exchange_data.keys()), 'Weighted Price')
```
![14.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514072697/rjed2dmup8di6pyk9zyc.png)

To check the values, use ```btc_usd_datasets.tail()```
![15.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514072715/patlocszclukqg9x8twm.png)

Plot the multiple exchanges price with date.
Use the code below:
```python
btc_trace_bitstamp = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['BITSTAMP'], name = 'BITSTAMP')
btc_trace_coinbase = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['COINBASE'], name = 'COINBASE')
btc_trace_itbit = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['ITBIT'], name = 'ITBIT')
btc_trace_kraken = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['KRAKEN'], name = 'KRAKEN')

py.plot([btc_trace_bitstamp, btc_trace_coinbase, btc_trace_itbit, btc_trace_kraken])
```
![16.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514100332/n6dtzh7nvtfw2ppdvxlz.png)

![17.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514100411/vca3zxdtuaqangdfrfqv.png)

In the chart above, we find that there are values *'zero'* for BTC which practically didn't exist. So, replace the values with *'NaN'* using the following syntax:
```
# Remove "0" values
btc_usd_datasets.replace(0, np.nan, inplace = True)
```
![19.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514100951/cxuwgn5cuzbvpnidaok0.png)

And  plot the new chart using 
```py.plot([btc_trace_bitstamp, btc_trace_coinbase, btc_trace_itbit, btc_trace_kraken])```
![18.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514100962/fuyn5aoy1ub0aueetduz.png)

To customize the chart more like adding **Title**, **Axes labels**,  **color**, **filename** ('temp.plot' by default)
The code for plotting the customized chart is as follows:
```python
data_plots = [btc_trace_bitstamp, btc_trace_coinbase, btc_trace_itbit, btc_trace_kraken]

# Customize the axes, title, color
layout_plots = go.Layout(
    title='Bitcoin Price (USD) By Exchange',
    xaxis=dict(
        title='Date',
        titlefont=dict(
            family='Courier New, monospace',
            size=18,
            color='#7f7f7f'
        )
    ),
    yaxis=dict(
        title='BTC Price',
        titlefont=dict(
            family='Courier New, monospace',
            size=18,
            color='#7f7f7f'
        )
    )
)

fig = go.Figure(data = data_plots, layout = layout_plots)
py.plot(fig, filename='Bitcoin Price (USD) By Exchange.html')
```
![20.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514101959/vddiym75wc141ihxzbdw.png)

![21.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514101987/dmmdt1zm57nen2zdowf5.png)

### Retrieve Altcoin Price data from POLONIEX exchange
**Altcoins** are cryptocurrencies other than **Bitcoin**. 
We will fetch the top 9 cryptocoins' price data.
And plot the chart - **'price for each crypto' vs 'date'**  & **a heatmap of all cryptos with each other.**

* **Define helper functions** - 
Code snippet for *'Helper function for getting JSON data'*:
```python
def get_json_data(json_url, cache_path):
    print('Downloading {}'.format(json_url))
    df = pd.get_json_data(json_url)
    df.to_pickle(cache_path)
    print('Cached response at {}'.format(cache_path))
    return df
```

Code snippet for *'Helper function for formatting the Poloniex API and get the JSON data using 'get_json_data' function'*:
```python
base_polo_url = 'https://poloniex.com/public?command=returnChartData&currencyPair={}&start={}&end={}&period={}'
start_date = time.mktime(datetime.strptime('2015-01-01', '%Y-%m-%d').timetuple()) # get data from the start of 2015 (i.e. 01-01-2015 in timestamp)
end_date = time.mktime(datetime.now().timetuple()) # up until today i.e. today in timestamp
period = 86400 # pull daily data (86,400 seconds per day)

def get_crypto_data(poloniex_pair):
    '''Retrieve altcoin's data using Poloniex API '''
    json_url = base_polo_url.format(poloniex_pair, start_date, end_date, period)
    data_df = get_json_data(json_url, poloniex_pair)
    data_df = data_df.set_index('date')
    return data_df
```
Here, the poloniex_pair can be e.g. BTC_ETH, BTC_XRP, etc.

* **Downloading data** - 
Code snippet:
```python
## Downloading the Trading data
altcoins = ['ETH', 'LTC', 'XRP', 'ETC', 'STR', 'DASH', 'SC', 'XMR', 'XEM']

altcoin_data = {}

for altcoin in altcoins:
    coinpair = 'BTC_{}'.format(altcoin)
    altcoin_data[altcoin] = get_crypto_data(coinpair)
```
![23.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514126766/b0nbqoh4ruucfobcrras.png)
Here, we are actually facing problem in fetching the data. It is due to the **reCAPTCHA** being asked each time. FYI, it is done for security reasons. Although, this reCAPTCHA issue can be resolved using [selenium](http://selenium-python.readthedocs.io/). I shall cover in another tutorial.

#### Solution: Either download manually from Poloniex or change the exchange where the data is given without captcha problem.

So, we need to download from the link manually e.g. 
**BTC_ETH**:  https://poloniex.com/public?command=returnChartData&currencyPair=BTC_ETH&start=1420050600.0&end=1514125175.0&period=86400 
**BTC_LTC**:  https://poloniex.com/public?command=returnChartData&currencyPair=BTC_LTC&start=1420050600.0&end=1514125175.0&period=86400 
.......and so on. 
right click and 'Save as' .json format. 

Now, in order to fetch the JSON data we will use pandas library.
I saved the file as 'poloniex_BTC_ETH.json' for coinpair - BTC_ETH
Code snippet:
```python
## Fetching the Trading data from json file into dataframe using pandas
altcoins = ['ETH', 'LTC', 'XRP', 'ETC', 'STR', 'DASH', 'SC', 'XMR', 'XEM']

altcoin_data = {}

for altcoin in altcoins:
    filename_json = 'poloniex_BTC_{}.json'.format(altcoin)
    altcoin_data[altcoin] = pd.read_json(filename_json)
```
![24.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514129311/ucpjxmxs17rwuxzdxdrq.png)

Visualize the data using ```altcoin_data['ETH'].tail()```
![25.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514129434/me6qibwpmapqe8doofqp.png)

Now we have a dictionary of 9 dataframes (for each crypto), each containing the historical daily average exchange prices between the **altcoin** and **Bitcoin**.
![26.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514129982/cvzbu8smxuu0vgesic51.png)

* **Convert altcoin's prices (in BTC, by default) to USD** - 
#### **ETH (price_usd) = ETH(price_btc) * BTC(price_usd)**
So, let's calculate the avg. BTC price using the code below:
```python
btc_usd_datasets['avg_btc_price_usd'] = btc_usd_datasets.mean(axis=1)
```
![27.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514133600/cwek85qnrkqx0p18msbj.png)

#### NOTE: for calculation of 2 dataframes, always ensure that the index col is same. In this case, index col is 'date'. Otherwise, one will find NaN values.
Now, for calculation, set 'date' as index column of 'altcoin_data'
```python
altcoin_data[altcoin] = altcoin_data[altcoin].set_index('date')  # set the column 'date' to index for further calculation.
```

The code snippet for calculating altcoin's price in USD is as follows:
```python
for altcoin in altcoin_data.keys():
    altcoin_data[altcoin]['price_usd'] = altcoin_data[altcoin]['weightedAverage'] * btc_usd_datasets['avg_btc_price_usd']
```

Visualize the data of 'altcoin_date' for 'ETH' and 'btc_usd_datasets' to see the values of altcoin in 'price_usd'

Few exchanges don't have the data for year 2011-2014. In order to compare the graphs for each altcoin we need to visualize b/w specific dates. **E.g. June 2015 to Dec 2017**
```python
# Visualize the data from 'June 2015' to 'Dec 2017'
btc_usd_datasets_2015to2017 = btc_usd_datasets[btc_usd_datasets.index.isin(pd.date_range('2015-06-01', '2017-12-21'))]
btc_usd_datasets_2015to2017
```
![28.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514186529/qzshxydhahfc4dsrsust.png)

```python
# Visualize the data from 'June 2015' to 'Dec 2017'
altcoin_data_2015to2017 = {}

for altcoin in altcoin_data.keys():
    altcoin_data_2015to2017[altcoin] = altcoin_data[altcoin][altcoin_data[altcoin].index.isin(pd.date_range('2015-06-01', '2017-12-21'))]

altcoin_data_2015to2017['ETH']
```
![29.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514186623/oyf16ftao8vsjyohg6rx.png)

Now, to combine the dataframes of 'altcoin_data' for each crypto, but only for 'price_usd' column:
```python
# Now, to combine the dataframes of 'altcoin_data' for each crypto, but only for 'price_usd' column
combined_df_2015to2017 = merge_dfs_on_column(list(altcoin_data_2015to2017.values()), list(altcoin_data_2015to2017.keys()), 'price_usd')
```
![30.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514187626/y2glca6m5yorb0tzuhpa.png)

Also, the 'BTC' column has been added below:
```python
# Add another column 'BTC' to the combined dataframe.
combined_df_2015to2017['BTC'] = btc_usd_datasets_2015to2017['avg_btc_price_usd']
```
![31.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514187767/ocdw5ayvltcddhaz9zri.png)

Now, simple plot is as follows:
Code snippet - 
```python
# Scatter plot for each crypto
altcoin_data_trace_dash = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['DASH'], name = 'DASH')
altcoin_data_trace_etc = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['ETC'], name = 'ETC')
altcoin_data_trace_eth = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['ETH'], name = 'ETH')
altcoin_data_trace_ltc = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['LTC'], name = 'LTC')
altcoin_data_trace_sc = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['SC'], name = 'SC')
altcoin_data_trace_str = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['STR'], name = 'STR')
altcoin_data_trace_xem = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['XEM'], name = 'XEM')
altcoin_data_trace_xmr = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['XMR'], name = 'XMR')
altcoin_data_trace_xrp = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['XRP'], name = 'XRP')
altcoin_data_trace_btc = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017['BTC'], name = 'BTC')

# combined array for scatter plots above
combined_df_data_trace_2015to2017 = [altcoin_data_trace_dash, altcoin_data_trace_etc, altcoin_data_trace_eth, altcoin_data_trace_ltc, altcoin_data_trace_sc, altcoin_data_trace_str, altcoin_data_trace_xem, altcoin_data_trace_xmr, altcoin_data_trace_xrp, altcoin_data_trace_btc]

py.plot(combined_df_data_trace_2015to2017)
```
![32.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514190540/s4h7i95j9bsxqbdecsf5.png)

![33.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514190574/ry7w8hqkphduzdwrti7c.png)

 Customize the plot - 
Code snippet:
```python
# Customize the axes, title, color
layout_plots = go.Layout(
    title='Cryptocurrency Prices (USD)',
    xaxis=dict(
        title='Date (June 2015 to Dec 2017)',
        titlefont=dict(
            family='Courier New, monospace',
            size=18,
            color='#7f7f7f'
        )
    ),
    yaxis=dict(
        title='Crypto Price (in USD)',
        titlefont=dict(
            family='Courier New, monospace',
            size=18,
            color='#7f7f7f'
        )
    )
)

fig = go.Figure(data = combined_df_data_trace_2015to2017, layout = layout_plots)
py.plot(fig, filename='Cryptocurrency Prices (USD).html')
```
![34.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514190789/yztloraiobmxvsrdnbxl.png)

Here, due to Bitcoin's high price, other graph seems negligible.

* **Correlations b/w cryptocurrencies** - 
Despite the exchangess price, the cryptocurrencies' price is slightly related. So, here we are going to find the correlation b/w cryptos.

We can test our correlation hypothesis using the Pandas corr() method, which computes a Pearson correlation coefficient for each column in the dataframe against each other column.

Code snippet :
```python
# Calculate the pearson correlation coefficients for altcoins in 2016
combined_df_2016 = combined_df_2015to2017[combined_df_2015to2017.index.year == 2016]
combined_df_2016.pct_change().corr(method='pearson')
```
![35.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514192047/sax8zov9mfroxn02rzcw.png)

Here, the table implies -
**Coefficient (close to 1 or -1)** - strongly correlated or inversely correlated.
**Coefficient (close to 0)** - values tend to fluctuate independently of each other.

The helper function is defined as:
```python
# Heatmap helper function
def correlation_heatmap(df, title, absolute_bounds=True):
    '''Plot a correlation heatmap for the entire dataframe'''
    heatmap = go.Heatmap(
        z=df.corr(method='pearson').as_matrix(),
        x=df.columns,
        y=df.columns,
        colorbar=dict(title='Pearson Coefficient'),
    )
    
    layout = go.Layout(title=title)
    
    if absolute_bounds:
        heatmap['zmax'] = 1.0
        heatmap['zmin'] = -1.0
        
    fig = go.Figure(data=[heatmap], layout=layout)
    py.plot(fig, filename=title+'.html')
```
The heatmap plotted using - 
```python
correlation_heatmap(combined_df_2016.pct_change(), "Cryptocurrency Correlations in 2016")
```
![36.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514193067/tyledwvoazeib3o3vtih.png)

Here, the dark red values represent strong correlations (note that each currency is, obviously, strongly correlated with itself), and the dark blue values represent strong inverse correlations. All of the light blue/orange/gray/tan colors in-between represent varying degrees of weak/non-existent correlations.

Let's see the graph of correlations in 2017:
![37.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514193329/rvk2tuwax43r7cqgqwdk.png)

![38.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514193346/rq9bfpjkvqhvmjf6hfss.png)
Huh. That's rather interesting!!!....

## Conclusion
The charts show the cryptocurrencies plots with the date. And we can enable notification feature which would notify us about the price's uptick.

At last, the heat map shows that how cryptocurrencies are inter-related as per their coefficient values. Application could be huge in case of trading. One can predict the graph based on other cryptos values.

Now, you can try for other cryptocurrencies as well.

Give it a shot!!...

That's all for now........

### Stay tuned for more such tutorials...

## View in [Steemit](https://steemit.com/utopian-io/@abhi3700/6rjxph-python-playground-2-crypto-analysis-of-bitcoin-altcoins-and-their-correlations-using-heatmap)

## View in [Utopian](https://utopian.io/utopian-io/@abhi3700/6rjxph-python-playground-2-crypto-analysis-of-bitcoin-altcoins-and-their-correlations-using-heatmap)

## Resources
* ANALYZING CRYPTOCURRENCY MARKETS USING PYTHON - https://blog.patricktriest.com/analyzing-cryptocurrencies-python/
