

```python
import os
import numpy as np
import pandas as pd
import pickle
import quandl
import time
from datetime import datetime
```


```python
import plotly.offline as py
import plotly.graph_objs as go
import plotly.figure_factory as ff
py.init_notebook_mode(connected = True)
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



```python
YOUR_QUANDL_API_KEY = "YTxiUfxz7vsTvr7yD8yt"
quandl.ApiConfig.api_key = YOUR_QUANDL_API_KEY
```


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


```python
btc_usd_price_kraken = get_quandl_data('BCHARTS/KRAKENUSD')
```

    Downloading BCHARTS/KRAKENUSD from Quandl
    Cached BCHARTS/KRAKENUSD at BCHARTS-KRAKENUSD.pkl
    


```python
btc_usd_price_kraken.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume (BTC)</th>
      <th>Volume (Currency)</th>
      <th>Weighted Price</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2014-01-07</th>
      <td>874.67040</td>
      <td>892.06753</td>
      <td>810.00000</td>
      <td>810.00000</td>
      <td>15.622378</td>
      <td>13151.472844</td>
      <td>841.835522</td>
    </tr>
    <tr>
      <th>2014-01-08</th>
      <td>810.00000</td>
      <td>899.84281</td>
      <td>788.00000</td>
      <td>824.98287</td>
      <td>19.182756</td>
      <td>16097.329584</td>
      <td>839.156269</td>
    </tr>
    <tr>
      <th>2014-01-09</th>
      <td>825.56345</td>
      <td>870.00000</td>
      <td>807.42084</td>
      <td>841.86934</td>
      <td>8.158335</td>
      <td>6784.249982</td>
      <td>831.572913</td>
    </tr>
    <tr>
      <th>2014-01-10</th>
      <td>839.99000</td>
      <td>857.34056</td>
      <td>817.00000</td>
      <td>857.33056</td>
      <td>8.024510</td>
      <td>6780.220188</td>
      <td>844.938794</td>
    </tr>
    <tr>
      <th>2014-01-11</th>
      <td>858.20000</td>
      <td>918.05471</td>
      <td>857.16554</td>
      <td>899.84105</td>
      <td>18.748285</td>
      <td>16698.566929</td>
      <td>890.671709</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Chart the BTC pricing data
btc_trace = go.Scatter(x = btc_usd_price_kraken.index, y = btc_usd_price_kraken['Weighted Price'])
```


```python
py.plot([btc_trace])
```




    'file://F:\\Coding\\Python\\Python playground Steemit\\cryptocurrency-analysis\\temp-plot.html'




```python
exchanges = ['COINBASE', 'BITSTAMP', 'ITBIT']
exchange_data = {}
exchange_data['KRAKEN'] = btc_usd_price_kraken

for exchange in exchanges:
    exchange_code = 'BCHARTS/{}USD'.format(exchange)
    exchange_data[exchange] = get_quandl_data(exchange_code)
```

    Downloading BCHARTS/COINBASEUSD from Quandl
    Cached BCHARTS/COINBASEUSD at BCHARTS-COINBASEUSD.pkl
    Downloading BCHARTS/BITSTAMPUSD from Quandl
    Cached BCHARTS/BITSTAMPUSD at BCHARTS-BITSTAMPUSD.pkl
    Downloading BCHARTS/ITBITUSD from Quandl
    Cached BCHARTS/ITBITUSD at BCHARTS-ITBITUSD.pkl
    


```python
exchange_data['COINBASE'].tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume (BTC)</th>
      <th>Volume (Currency)</th>
      <th>Weighted Price</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2017-12-17</th>
      <td>19650.02</td>
      <td>19891.99</td>
      <td>19010.0</td>
      <td>19378.99</td>
      <td>21169.979725</td>
      <td>4.118753e+08</td>
      <td>19455.628104</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>19378.99</td>
      <td>19385.00</td>
      <td>18200.0</td>
      <td>19039.01</td>
      <td>25749.173669</td>
      <td>4.835641e+08</td>
      <td>18779.790859</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>19039.01</td>
      <td>19099.00</td>
      <td>16800.0</td>
      <td>17838.73</td>
      <td>31731.286114</td>
      <td>5.726482e+08</td>
      <td>18046.801749</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>17838.73</td>
      <td>17890.00</td>
      <td>14001.0</td>
      <td>16496.89</td>
      <td>47290.587499</td>
      <td>7.863248e+08</td>
      <td>16627.512506</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>16496.90</td>
      <td>17364.57</td>
      <td>15151.0</td>
      <td>15726.71</td>
      <td>28493.978442</td>
      <td>4.579575e+08</td>
      <td>16072.079918</td>
    </tr>
  </tbody>
</table>
</div>




```python
exchange_data['BITSTAMP'].tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume (BTC)</th>
      <th>Volume (Currency)</th>
      <th>Weighted Price</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2017-12-18</th>
      <td>18953.00</td>
      <td>19220.00</td>
      <td>17835.20</td>
      <td>18940.57</td>
      <td>14678.941448</td>
      <td>2.735335e+08</td>
      <td>18634.418590</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>18940.58</td>
      <td>19160.79</td>
      <td>16831.26</td>
      <td>17700.00</td>
      <td>21528.137665</td>
      <td>3.872062e+08</td>
      <td>17986.053806</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>17700.00</td>
      <td>17950.00</td>
      <td>15343.04</td>
      <td>16466.98</td>
      <td>31172.228807</td>
      <td>5.213506e+08</td>
      <td>16724.840365</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>16466.98</td>
      <td>17281.17</td>
      <td>15005.00</td>
      <td>15600.01</td>
      <td>20377.860439</td>
      <td>3.267307e+08</td>
      <td>16033.611116</td>
    </tr>
    <tr>
      <th>2017-12-22</th>
      <td>15600.00</td>
      <td>15743.71</td>
      <td>15600.00</td>
      <td>15716.93</td>
      <td>50.775202</td>
      <td>7.957203e+05</td>
      <td>15671.434483</td>
    </tr>
  </tbody>
</table>
</div>




```python
exchange_data['ITBIT'].tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume (BTC)</th>
      <th>Volume (Currency)</th>
      <th>Weighted Price</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2017-12-17</th>
      <td>19347.71</td>
      <td>19720.00</td>
      <td>18758.85</td>
      <td>19050.00</td>
      <td>674.4542</td>
      <td>1.298141e+07</td>
      <td>19247.277924</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>19055.00</td>
      <td>19230.24</td>
      <td>18079.00</td>
      <td>18930.81</td>
      <td>1687.7579</td>
      <td>3.146039e+07</td>
      <td>18640.346919</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>18934.39</td>
      <td>18996.00</td>
      <td>16725.87</td>
      <td>17596.10</td>
      <td>1756.0477</td>
      <td>3.149441e+07</td>
      <td>17934.823780</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>17602.50</td>
      <td>17850.00</td>
      <td>15544.43</td>
      <td>16473.97</td>
      <td>3170.0330</td>
      <td>5.276436e+07</td>
      <td>16644.736331</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>16514.25</td>
      <td>17279.55</td>
      <td>15076.52</td>
      <td>15288.94</td>
      <td>2089.1958</td>
      <td>3.330239e+07</td>
      <td>15940.292149</td>
    </tr>
  </tbody>
</table>
</div>




```python
list(exchange_data.keys())
```




    ['KRAKEN', 'COINBASE', 'BITSTAMP', 'ITBIT']




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
btc_usd_datasets = merge_dfs_on_column(list(exchange_data.values()), list(exchange_data.keys()), 'Weighted Price')
```


```python
btc_usd_datasets.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BITSTAMP</th>
      <th>COINBASE</th>
      <th>ITBIT</th>
      <th>KRAKEN</th>
      <th>avg_btc_price_usd</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2011-09-13</th>
      <td>5.929231</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.929231</td>
    </tr>
    <tr>
      <th>2011-09-14</th>
      <td>5.590798</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.590798</td>
    </tr>
    <tr>
      <th>2011-09-15</th>
      <td>5.094272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.094272</td>
    </tr>
    <tr>
      <th>2011-09-16</th>
      <td>4.854515</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.854515</td>
    </tr>
    <tr>
      <th>2011-09-17</th>
      <td>4.870000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.870000</td>
    </tr>
  </tbody>
</table>
</div>




```python
list(btc_usd_datasets)
```




    ['BITSTAMP', 'COINBASE', 'ITBIT', 'KRAKEN']




```python
btc_trace_bitstamp = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['BITSTAMP'], name = 'BITSTAMP')
btc_trace_coinbase = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['COINBASE'], name = 'COINBASE')
btc_trace_itbit = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['ITBIT'], name = 'ITBIT')
btc_trace_kraken = go.Scatter(x = btc_usd_datasets.index, y = btc_usd_datasets['KRAKEN'], name = 'KRAKEN')

py.plot([btc_trace_bitstamp, btc_trace_coinbase, btc_trace_itbit, btc_trace_kraken])
```




    'file://F:\\Coding\\Python\\Python playground Steemit\\cryptocurrency-analysis\\temp-plot.html'




```python
# Remove "0" values
btc_usd_datasets.replace(0, np.nan, inplace = True)
```


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




    'file://F:\\Coding\\Python\\Python playground Steemit\\cryptocurrency-analysis\\Bitcoin Price (USD) By Exchange.html'




```python
# Altcoin price from POLONIEX exchange

## Helper function for getting JSON data
def get_json_data(json_url, cache_path):
    print('Downloading {}'.format(json_url))
    df = pd.read_json(json_url)
    df.to_pickle(cache_path)
    print('Cached response at {}'.format(cache_path))
    return df

## Helper function for formatting the Poloniex API and get the JSON data using previous function
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


```python
## Fetching the Trading data from json file into dataframe using pandas
altcoins = ['ETH', 'LTC', 'XRP', 'ETC', 'STR', 'DASH', 'SC', 'XMR', 'XEM']

altcoin_data = {}

for altcoin in altcoins:
    filename_json = 'poloniex_BTC_{}.json'.format(altcoin)
    altcoin_data[altcoin] = pd.read_json(filename_json)
    altcoin_data[altcoin] = altcoin_data[altcoin].set_index('date')  # set the column 'date' to index for further calculation.
```


```python
altcoin_data['ETH']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>close</th>
      <th>high</th>
      <th>low</th>
      <th>open</th>
      <th>quoteVolume</th>
      <th>volume</th>
      <th>weightedAverage</th>
      <th>price_usd</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-08-08</th>
      <td>0.003125</td>
      <td>50.000000</td>
      <td>0.002620</td>
      <td>50.000000</td>
      <td>2.662061e+05</td>
      <td>1205.803321</td>
      <td>0.004530</td>
      <td>1.224162</td>
    </tr>
    <tr>
      <th>2015-08-09</th>
      <td>0.002581</td>
      <td>0.004100</td>
      <td>0.002400</td>
      <td>0.003000</td>
      <td>3.139879e+05</td>
      <td>898.123434</td>
      <td>0.002860</td>
      <td>0.755301</td>
    </tr>
    <tr>
      <th>2015-08-10</th>
      <td>0.002645</td>
      <td>0.002902</td>
      <td>0.002200</td>
      <td>0.002650</td>
      <td>2.845754e+05</td>
      <td>718.365266</td>
      <td>0.002524</td>
      <td>0.669491</td>
    </tr>
    <tr>
      <th>2015-08-11</th>
      <td>0.003950</td>
      <td>0.004400</td>
      <td>0.002414</td>
      <td>0.002650</td>
      <td>9.151385e+05</td>
      <td>3007.274111</td>
      <td>0.003286</td>
      <td>0.874050</td>
    </tr>
    <tr>
      <th>2015-08-12</th>
      <td>0.004500</td>
      <td>0.004882</td>
      <td>0.002910</td>
      <td>0.003955</td>
      <td>1.117821e+06</td>
      <td>4690.075032</td>
      <td>0.004196</td>
      <td>1.127745</td>
    </tr>
    <tr>
      <th>2015-08-13</th>
      <td>0.006955</td>
      <td>0.007999</td>
      <td>0.004010</td>
      <td>0.004500</td>
      <td>1.361099e+06</td>
      <td>8192.675011</td>
      <td>0.006019</td>
      <td>1.596377</td>
    </tr>
    <tr>
      <th>2015-08-14</th>
      <td>0.006831</td>
      <td>0.008888</td>
      <td>0.006500</td>
      <td>0.006880</td>
      <td>1.531205e+06</td>
      <td>11651.548987</td>
      <td>0.007609</td>
      <td>2.021394</td>
    </tr>
    <tr>
      <th>2015-08-15</th>
      <td>0.006377</td>
      <td>0.007205</td>
      <td>0.005700</td>
      <td>0.006858</td>
      <td>7.361567e+05</td>
      <td>4745.484895</td>
      <td>0.006446</td>
      <td>1.712331</td>
    </tr>
    <tr>
      <th>2015-08-16</th>
      <td>0.006190</td>
      <td>0.006559</td>
      <td>0.004150</td>
      <td>0.006377</td>
      <td>1.469308e+06</td>
      <td>7666.273099</td>
      <td>0.005218</td>
      <td>1.354970</td>
    </tr>
    <tr>
      <th>2015-08-17</th>
      <td>0.004747</td>
      <td>0.006195</td>
      <td>0.004395</td>
      <td>0.006190</td>
      <td>8.906068e+05</td>
      <td>4618.675968</td>
      <td>0.005186</td>
      <td>1.341226</td>
    </tr>
    <tr>
      <th>2015-08-18</th>
      <td>0.005139</td>
      <td>0.005372</td>
      <td>0.004251</td>
      <td>0.004685</td>
      <td>8.989845e+05</td>
      <td>4321.566218</td>
      <td>0.004807</td>
      <td>1.178080</td>
    </tr>
    <tr>
      <th>2015-08-19</th>
      <td>0.005500</td>
      <td>0.005700</td>
      <td>0.005005</td>
      <td>0.005140</td>
      <td>6.890972e+05</td>
      <td>3728.056758</td>
      <td>0.005410</td>
      <td>1.254231</td>
    </tr>
    <tr>
      <th>2015-08-20</th>
      <td>0.006220</td>
      <td>0.006570</td>
      <td>0.005228</td>
      <td>0.005500</td>
      <td>1.229657e+06</td>
      <td>7236.153172</td>
      <td>0.005885</td>
      <td>1.375320</td>
    </tr>
    <tr>
      <th>2015-08-21</th>
      <td>0.005959</td>
      <td>0.006777</td>
      <td>0.005711</td>
      <td>0.006250</td>
      <td>9.009119e+05</td>
      <td>5677.528871</td>
      <td>0.006302</td>
      <td>1.475977</td>
    </tr>
    <tr>
      <th>2015-08-22</th>
      <td>0.005953</td>
      <td>0.006490</td>
      <td>0.005805</td>
      <td>0.005959</td>
      <td>3.704242e+05</td>
      <td>2285.268266</td>
      <td>0.006169</td>
      <td>1.407928</td>
    </tr>
    <tr>
      <th>2015-08-23</th>
      <td>0.005889</td>
      <td>0.006358</td>
      <td>0.005599</td>
      <td>0.005953</td>
      <td>7.356438e+05</td>
      <td>4325.617489</td>
      <td>0.005880</td>
      <td>1.350845</td>
    </tr>
    <tr>
      <th>2015-08-24</th>
      <td>0.005834</td>
      <td>0.006145</td>
      <td>0.005770</td>
      <td>0.005959</td>
      <td>3.193668e+05</td>
      <td>1887.470634</td>
      <td>0.005910</td>
      <td>1.289791</td>
    </tr>
    <tr>
      <th>2015-08-25</th>
      <td>0.005134</td>
      <td>0.005942</td>
      <td>0.005000</td>
      <td>0.005834</td>
      <td>6.256742e+05</td>
      <td>3373.828192</td>
      <td>0.005392</td>
      <td>1.158191</td>
    </tr>
    <tr>
      <th>2015-08-26</th>
      <td>0.005190</td>
      <td>0.005284</td>
      <td>0.004773</td>
      <td>0.005134</td>
      <td>4.877377e+05</td>
      <td>2463.805429</td>
      <td>0.005051</td>
      <td>1.151170</td>
    </tr>
    <tr>
      <th>2015-08-27</th>
      <td>0.005100</td>
      <td>0.005275</td>
      <td>0.004984</td>
      <td>0.005135</td>
      <td>2.720249e+05</td>
      <td>1399.600139</td>
      <td>0.005145</td>
      <td>1.167442</td>
    </tr>
    <tr>
      <th>2015-08-28</th>
      <td>0.005100</td>
      <td>0.005230</td>
      <td>0.005010</td>
      <td>0.005100</td>
      <td>1.990655e+05</td>
      <td>1020.524557</td>
      <td>0.005127</td>
      <td>1.177989</td>
    </tr>
    <tr>
      <th>2015-08-29</th>
      <td>0.005145</td>
      <td>0.005200</td>
      <td>0.005000</td>
      <td>0.005100</td>
      <td>1.802156e+05</td>
      <td>918.455330</td>
      <td>0.005096</td>
      <td>1.176311</td>
    </tr>
    <tr>
      <th>2015-08-30</th>
      <td>0.005750</td>
      <td>0.006051</td>
      <td>0.005065</td>
      <td>0.005145</td>
      <td>6.004443e+05</td>
      <td>3426.154790</td>
      <td>0.005706</td>
      <td>1.306319</td>
    </tr>
    <tr>
      <th>2015-08-31</th>
      <td>0.005912</td>
      <td>0.006350</td>
      <td>0.005226</td>
      <td>0.005750</td>
      <td>6.401329e+05</td>
      <td>3731.085653</td>
      <td>0.005829</td>
      <td>1.336580</td>
    </tr>
    <tr>
      <th>2015-09-01</th>
      <td>0.005924</td>
      <td>0.006140</td>
      <td>0.005817</td>
      <td>0.005900</td>
      <td>3.185666e+05</td>
      <td>1907.926470</td>
      <td>0.005989</td>
      <td>1.368114</td>
    </tr>
    <tr>
      <th>2015-09-02</th>
      <td>0.005660</td>
      <td>0.005968</td>
      <td>0.005459</td>
      <td>0.005924</td>
      <td>3.743889e+05</td>
      <td>2147.969002</td>
      <td>0.005737</td>
      <td>1.312541</td>
    </tr>
    <tr>
      <th>2015-09-03</th>
      <td>0.005500</td>
      <td>0.005739</td>
      <td>0.005204</td>
      <td>0.005658</td>
      <td>3.319758e+05</td>
      <td>1806.284582</td>
      <td>0.005441</td>
      <td>1.240465</td>
    </tr>
    <tr>
      <th>2015-09-04</th>
      <td>0.005509</td>
      <td>0.005732</td>
      <td>0.005355</td>
      <td>0.005500</td>
      <td>2.139500e+05</td>
      <td>1190.493443</td>
      <td>0.005564</td>
      <td>1.278176</td>
    </tr>
    <tr>
      <th>2015-09-05</th>
      <td>0.005699</td>
      <td>0.005730</td>
      <td>0.005500</td>
      <td>0.005510</td>
      <td>1.214705e+05</td>
      <td>683.539477</td>
      <td>0.005627</td>
      <td>1.316373</td>
    </tr>
    <tr>
      <th>2015-09-06</th>
      <td>0.005383</td>
      <td>0.005740</td>
      <td>0.005369</td>
      <td>0.005650</td>
      <td>1.524807e+05</td>
      <td>838.524738</td>
      <td>0.005499</td>
      <td>1.322887</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-11-25</th>
      <td>0.052845</td>
      <td>0.059121</td>
      <td>0.052845</td>
      <td>0.057462</td>
      <td>1.878513e+05</td>
      <td>10450.760172</td>
      <td>0.055633</td>
      <td>472.301796</td>
    </tr>
    <tr>
      <th>2017-11-26</th>
      <td>0.050586</td>
      <td>0.052994</td>
      <td>0.047662</td>
      <td>0.052845</td>
      <td>2.027793e+05</td>
      <td>10231.093743</td>
      <td>0.050454</td>
      <td>459.673489</td>
    </tr>
    <tr>
      <th>2017-11-27</th>
      <td>0.048807</td>
      <td>0.050900</td>
      <td>0.048500</td>
      <td>0.050586</td>
      <td>1.175349e+05</td>
      <td>5819.785201</td>
      <td>0.049515</td>
      <td>473.693035</td>
    </tr>
    <tr>
      <th>2017-11-28</th>
      <td>0.046956</td>
      <td>0.049416</td>
      <td>0.046336</td>
      <td>0.048716</td>
      <td>1.296472e+05</td>
      <td>6185.746635</td>
      <td>0.047712</td>
      <td>468.886236</td>
    </tr>
    <tr>
      <th>2017-11-29</th>
      <td>0.043200</td>
      <td>0.047906</td>
      <td>0.043047</td>
      <td>0.046956</td>
      <td>2.604961e+05</td>
      <td>11763.896535</td>
      <td>0.045160</td>
      <td>467.703173</td>
    </tr>
    <tr>
      <th>2017-11-30</th>
      <td>0.043770</td>
      <td>0.044845</td>
      <td>0.042450</td>
      <td>0.043300</td>
      <td>1.331669e+05</td>
      <td>5801.811208</td>
      <td>0.043568</td>
      <td>427.562100</td>
    </tr>
    <tr>
      <th>2017-12-01</th>
      <td>0.042447</td>
      <td>0.044571</td>
      <td>0.042202</td>
      <td>0.043615</td>
      <td>8.286531e+04</td>
      <td>3589.518042</td>
      <td>0.043317</td>
      <td>446.614775</td>
    </tr>
    <tr>
      <th>2017-12-02</th>
      <td>0.041811</td>
      <td>0.042657</td>
      <td>0.041600</td>
      <td>0.042447</td>
      <td>5.906807e+04</td>
      <td>2484.743072</td>
      <td>0.042066</td>
      <td>459.889226</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>0.041286</td>
      <td>0.042200</td>
      <td>0.040400</td>
      <td>0.041966</td>
      <td>9.070096e+04</td>
      <td>3744.967920</td>
      <td>0.041289</td>
      <td>465.363326</td>
    </tr>
    <tr>
      <th>2017-12-04</th>
      <td>0.040090</td>
      <td>0.041247</td>
      <td>0.039802</td>
      <td>0.041120</td>
      <td>6.667896e+04</td>
      <td>2702.009705</td>
      <td>0.040523</td>
      <td>460.075398</td>
    </tr>
    <tr>
      <th>2017-12-05</th>
      <td>0.038974</td>
      <td>0.040430</td>
      <td>0.038500</td>
      <td>0.040090</td>
      <td>8.192597e+04</td>
      <td>3220.563718</td>
      <td>0.039311</td>
      <td>459.345558</td>
    </tr>
    <tr>
      <th>2017-12-06</th>
      <td>0.030330</td>
      <td>0.038966</td>
      <td>0.029003</td>
      <td>0.038801</td>
      <td>3.375624e+05</td>
      <td>11393.113521</td>
      <td>0.033751</td>
      <td>429.649880</td>
    </tr>
    <tr>
      <th>2017-12-07</th>
      <td>0.024883</td>
      <td>0.032321</td>
      <td>0.024714</td>
      <td>0.030330</td>
      <td>4.458287e+05</td>
      <td>12328.163369</td>
      <td>0.027652</td>
      <td>418.813570</td>
    </tr>
    <tr>
      <th>2017-12-08</th>
      <td>0.027759</td>
      <td>0.031700</td>
      <td>0.023700</td>
      <td>0.024883</td>
      <td>4.798754e+05</td>
      <td>13334.945566</td>
      <td>0.027788</td>
      <td>433.620825</td>
    </tr>
    <tr>
      <th>2017-12-09</th>
      <td>0.031287</td>
      <td>0.034813</td>
      <td>0.027500</td>
      <td>0.027880</td>
      <td>2.832214e+05</td>
      <td>8972.838448</td>
      <td>0.031681</td>
      <td>468.839547</td>
    </tr>
    <tr>
      <th>2017-12-10</th>
      <td>0.028639</td>
      <td>0.032980</td>
      <td>0.027783</td>
      <td>0.031304</td>
      <td>2.060106e+05</td>
      <td>6245.219863</td>
      <td>0.030315</td>
      <td>441.076198</td>
    </tr>
    <tr>
      <th>2017-12-11</th>
      <td>0.030700</td>
      <td>0.031085</td>
      <td>0.026823</td>
      <td>0.028636</td>
      <td>1.911329e+05</td>
      <td>5433.180530</td>
      <td>0.028426</td>
      <td>470.455430</td>
    </tr>
    <tr>
      <th>2017-12-12</th>
      <td>0.037440</td>
      <td>0.039023</td>
      <td>0.029710</td>
      <td>0.030710</td>
      <td>4.582798e+05</td>
      <td>15686.724043</td>
      <td>0.034230</td>
      <td>581.785276</td>
    </tr>
    <tr>
      <th>2017-12-13</th>
      <td>0.042715</td>
      <td>0.043738</td>
      <td>0.036000</td>
      <td>0.037706</td>
      <td>3.943022e+05</td>
      <td>15872.300955</td>
      <td>0.040254</td>
      <td>672.087409</td>
    </tr>
    <tr>
      <th>2017-12-14</th>
      <td>0.041824</td>
      <td>0.046063</td>
      <td>0.040000</td>
      <td>0.042880</td>
      <td>3.015209e+05</td>
      <td>12936.404634</td>
      <td>0.042904</td>
      <td>707.430304</td>
    </tr>
    <tr>
      <th>2017-12-15</th>
      <td>0.038701</td>
      <td>0.042100</td>
      <td>0.034300</td>
      <td>0.041824</td>
      <td>2.424363e+05</td>
      <td>9156.088428</td>
      <td>0.037767</td>
      <td>657.368350</td>
    </tr>
    <tr>
      <th>2017-12-16</th>
      <td>0.035485</td>
      <td>0.040799</td>
      <td>0.034772</td>
      <td>0.038701</td>
      <td>1.638981e+05</td>
      <td>6153.492990</td>
      <td>0.037545</td>
      <td>696.904375</td>
    </tr>
    <tr>
      <th>2017-12-17</th>
      <td>0.037600</td>
      <td>0.038620</td>
      <td>0.035490</td>
      <td>0.035501</td>
      <td>1.391539e+05</td>
      <td>5153.693210</td>
      <td>0.037036</td>
      <td>712.465534</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>0.041560</td>
      <td>0.042915</td>
      <td>0.037124</td>
      <td>0.037600</td>
      <td>1.735592e+05</td>
      <td>6906.643679</td>
      <td>0.039794</td>
      <td>743.536101</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>0.046195</td>
      <td>0.047125</td>
      <td>0.040800</td>
      <td>0.041400</td>
      <td>2.521555e+05</td>
      <td>11360.369368</td>
      <td>0.045053</td>
      <td>811.639229</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>0.048405</td>
      <td>0.049291</td>
      <td>0.044710</td>
      <td>0.046195</td>
      <td>2.324343e+05</td>
      <td>10913.157781</td>
      <td>0.046952</td>
      <td>780.718146</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>0.050461</td>
      <td>0.051600</td>
      <td>0.047519</td>
      <td>0.048400</td>
      <td>1.884621e+05</td>
      <td>9360.662383</td>
      <td>0.049669</td>
      <td>791.643752</td>
    </tr>
    <tr>
      <th>2017-12-22</th>
      <td>0.047730</td>
      <td>0.051707</td>
      <td>0.045229</td>
      <td>0.050364</td>
      <td>2.847822e+05</td>
      <td>13802.459027</td>
      <td>0.048467</td>
      <td>748.259744</td>
    </tr>
    <tr>
      <th>2017-12-23</th>
      <td>0.048187</td>
      <td>0.049048</td>
      <td>0.047244</td>
      <td>0.047730</td>
      <td>9.158762e+04</td>
      <td>4420.808319</td>
      <td>0.048269</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2017-12-24</th>
      <td>0.048209</td>
      <td>0.048732</td>
      <td>0.047100</td>
      <td>0.048100</td>
      <td>6.285688e+04</td>
      <td>3019.504658</td>
      <td>0.048038</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>870 rows × 8 columns</p>
</div>




```python
altcoin_data.values()
```




    dict_values([        close       date       high       low       open   quoteVolume  \
    0    0.003125 2015-08-08  50.000000  0.002620  50.000000  2.662061e+05   
    1    0.002581 2015-08-09   0.004100  0.002400   0.003000  3.139879e+05   
    2    0.002645 2015-08-10   0.002902  0.002200   0.002650  2.845754e+05   
    3    0.003950 2015-08-11   0.004400  0.002414   0.002650  9.151385e+05   
    4    0.004500 2015-08-12   0.004882  0.002910   0.003955  1.117821e+06   
    5    0.006955 2015-08-13   0.007999  0.004010   0.004500  1.361099e+06   
    6    0.006831 2015-08-14   0.008888  0.006500   0.006880  1.531205e+06   
    7    0.006377 2015-08-15   0.007205  0.005700   0.006858  7.361567e+05   
    8    0.006190 2015-08-16   0.006559  0.004150   0.006377  1.469308e+06   
    9    0.004747 2015-08-17   0.006195  0.004395   0.006190  8.906068e+05   
    10   0.005139 2015-08-18   0.005372  0.004251   0.004685  8.989845e+05   
    11   0.005500 2015-08-19   0.005700  0.005005   0.005140  6.890972e+05   
    12   0.006220 2015-08-20   0.006570  0.005228   0.005500  1.229657e+06   
    13   0.005959 2015-08-21   0.006777  0.005711   0.006250  9.009119e+05   
    14   0.005953 2015-08-22   0.006490  0.005805   0.005959  3.704242e+05   
    15   0.005889 2015-08-23   0.006358  0.005599   0.005953  7.356438e+05   
    16   0.005834 2015-08-24   0.006145  0.005770   0.005959  3.193668e+05   
    17   0.005134 2015-08-25   0.005942  0.005000   0.005834  6.256742e+05   
    18   0.005190 2015-08-26   0.005284  0.004773   0.005134  4.877377e+05   
    19   0.005100 2015-08-27   0.005275  0.004984   0.005135  2.720249e+05   
    20   0.005100 2015-08-28   0.005230  0.005010   0.005100  1.990655e+05   
    21   0.005145 2015-08-29   0.005200  0.005000   0.005100  1.802156e+05   
    22   0.005750 2015-08-30   0.006051  0.005065   0.005145  6.004443e+05   
    23   0.005912 2015-08-31   0.006350  0.005226   0.005750  6.401329e+05   
    24   0.005924 2015-09-01   0.006140  0.005817   0.005900  3.185666e+05   
    25   0.005660 2015-09-02   0.005968  0.005459   0.005924  3.743889e+05   
    26   0.005500 2015-09-03   0.005739  0.005204   0.005658  3.319758e+05   
    27   0.005509 2015-09-04   0.005732  0.005355   0.005500  2.139500e+05   
    28   0.005699 2015-09-05   0.005730  0.005500   0.005510  1.214705e+05   
    29   0.005383 2015-09-06   0.005740  0.005369   0.005650  1.524807e+05   
    ..        ...        ...        ...       ...        ...           ...   
    840  0.052845 2017-11-25   0.059121  0.052845   0.057462  1.878513e+05   
    841  0.050586 2017-11-26   0.052994  0.047662   0.052845  2.027793e+05   
    842  0.048807 2017-11-27   0.050900  0.048500   0.050586  1.175349e+05   
    843  0.046956 2017-11-28   0.049416  0.046336   0.048716  1.296472e+05   
    844  0.043200 2017-11-29   0.047906  0.043047   0.046956  2.604961e+05   
    845  0.043770 2017-11-30   0.044845  0.042450   0.043300  1.331669e+05   
    846  0.042447 2017-12-01   0.044571  0.042202   0.043615  8.286531e+04   
    847  0.041811 2017-12-02   0.042657  0.041600   0.042447  5.906807e+04   
    848  0.041286 2017-12-03   0.042200  0.040400   0.041966  9.070096e+04   
    849  0.040090 2017-12-04   0.041247  0.039802   0.041120  6.667896e+04   
    850  0.038974 2017-12-05   0.040430  0.038500   0.040090  8.192597e+04   
    851  0.030330 2017-12-06   0.038966  0.029003   0.038801  3.375624e+05   
    852  0.024883 2017-12-07   0.032321  0.024714   0.030330  4.458287e+05   
    853  0.027759 2017-12-08   0.031700  0.023700   0.024883  4.798754e+05   
    854  0.031287 2017-12-09   0.034813  0.027500   0.027880  2.832214e+05   
    855  0.028639 2017-12-10   0.032980  0.027783   0.031304  2.060106e+05   
    856  0.030700 2017-12-11   0.031085  0.026823   0.028636  1.911329e+05   
    857  0.037440 2017-12-12   0.039023  0.029710   0.030710  4.582798e+05   
    858  0.042715 2017-12-13   0.043738  0.036000   0.037706  3.943022e+05   
    859  0.041824 2017-12-14   0.046063  0.040000   0.042880  3.015209e+05   
    860  0.038701 2017-12-15   0.042100  0.034300   0.041824  2.424363e+05   
    861  0.035485 2017-12-16   0.040799  0.034772   0.038701  1.638981e+05   
    862  0.037600 2017-12-17   0.038620  0.035490   0.035501  1.391539e+05   
    863  0.041560 2017-12-18   0.042915  0.037124   0.037600  1.735592e+05   
    864  0.046195 2017-12-19   0.047125  0.040800   0.041400  2.521555e+05   
    865  0.048405 2017-12-20   0.049291  0.044710   0.046195  2.324343e+05   
    866  0.050461 2017-12-21   0.051600  0.047519   0.048400  1.884621e+05   
    867  0.047730 2017-12-22   0.051707  0.045229   0.050364  2.847822e+05   
    868  0.048187 2017-12-23   0.049048  0.047244   0.047730  9.158762e+04   
    869  0.048209 2017-12-24   0.048732  0.047100   0.048100  6.285688e+04   
    
               volume  weightedAverage  
    0     1205.803321         0.004530  
    1      898.123434         0.002860  
    2      718.365266         0.002524  
    3     3007.274111         0.003286  
    4     4690.075032         0.004196  
    5     8192.675011         0.006019  
    6    11651.548987         0.007609  
    7     4745.484895         0.006446  
    8     7666.273099         0.005218  
    9     4618.675968         0.005186  
    10    4321.566218         0.004807  
    11    3728.056758         0.005410  
    12    7236.153172         0.005885  
    13    5677.528871         0.006302  
    14    2285.268266         0.006169  
    15    4325.617489         0.005880  
    16    1887.470634         0.005910  
    17    3373.828192         0.005392  
    18    2463.805429         0.005051  
    19    1399.600139         0.005145  
    20    1020.524557         0.005127  
    21     918.455330         0.005096  
    22    3426.154790         0.005706  
    23    3731.085653         0.005829  
    24    1907.926470         0.005989  
    25    2147.969002         0.005737  
    26    1806.284582         0.005441  
    27    1190.493443         0.005564  
    28     683.539477         0.005627  
    29     838.524738         0.005499  
    ..            ...              ...  
    840  10450.760172         0.055633  
    841  10231.093743         0.050454  
    842   5819.785201         0.049515  
    843   6185.746635         0.047712  
    844  11763.896535         0.045160  
    845   5801.811208         0.043568  
    846   3589.518042         0.043317  
    847   2484.743072         0.042066  
    848   3744.967920         0.041289  
    849   2702.009705         0.040523  
    850   3220.563718         0.039311  
    851  11393.113521         0.033751  
    852  12328.163369         0.027652  
    853  13334.945566         0.027788  
    854   8972.838448         0.031681  
    855   6245.219863         0.030315  
    856   5433.180530         0.028426  
    857  15686.724043         0.034230  
    858  15872.300955         0.040254  
    859  12936.404634         0.042904  
    860   9156.088428         0.037767  
    861   6153.492990         0.037545  
    862   5153.693210         0.037036  
    863   6906.643679         0.039794  
    864  11360.369368         0.045053  
    865  10913.157781         0.046952  
    866   9360.662383         0.049669  
    867  13802.459027         0.048467  
    868   4420.808319         0.048269  
    869   3019.504658         0.048038  
    
    [870 rows x 8 columns],          close       date      high       low      open   quoteVolume  \
    0     0.008614 2015-01-01  0.008688  0.008450  0.008543  1.934275e+02   
    1     0.008410 2015-01-02  0.008616  0.008400  0.008614  3.385980e+02   
    2     0.007630 2015-01-03  0.008570  0.007530  0.008454  1.655027e+03   
    3     0.007549 2015-01-04  0.007999  0.007301  0.007632  1.155307e+03   
    4     0.007770 2015-01-05  0.007800  0.007410  0.007410  7.501571e+02   
    5     0.007695 2015-01-06  0.007769  0.007524  0.007700  4.137631e+02   
    6     0.007310 2015-01-07  0.007683  0.007310  0.007525  5.338007e+02   
    7     0.007276 2015-01-08  0.007560  0.007012  0.007308  9.424009e+02   
    8     0.006876 2015-01-09  0.007257  0.006872  0.007257  5.952354e+02   
    9     0.006223 2015-01-10  0.007056  0.006003  0.006887  7.542096e+02   
    10    0.006693 2015-01-11  0.006950  0.006158  0.006202  2.015988e+03   
    11    0.006313 2015-01-12  0.006873  0.006313  0.006693  6.797729e+02   
    12    0.006902 2015-01-13  0.007100  0.006130  0.006400  1.217910e+03   
    13    0.006555 2015-01-14  0.007591  0.006500  0.007100  1.555923e+03   
    14    0.006184 2015-01-15  0.006756  0.006000  0.006548  1.237507e+03   
    15    0.006725 2015-01-16  0.007076  0.006169  0.006340  1.011947e+03   
    16    0.006652 2015-01-17  0.006879  0.006500  0.006879  2.528074e+03   
    17    0.006333 2015-01-18  0.006675  0.006300  0.006533  5.035834e+02   
    18    0.006393 2015-01-19  0.006451  0.006000  0.006400  1.256139e+03   
    19    0.006384 2015-01-20  0.006451  0.006215  0.006390  1.122591e+03   
    20    0.006178 2015-01-21  0.006400  0.006178  0.006336  4.403931e+02   
    21    0.006082 2015-01-22  0.006392  0.006056  0.006160  5.158202e+02   
    22    0.006165 2015-01-23  0.006378  0.006084  0.006084  5.057802e+02   
    23    0.007601 2015-01-24  0.007682  0.006149  0.006149  2.261941e+03   
    24    0.008870 2015-01-25  0.009370  0.007473  0.007601  4.553174e+03   
    25    0.007902 2015-01-26  0.009200  0.007551  0.008977  5.461531e+03   
    26    0.008067 2015-01-27  0.008300  0.007585  0.007727  1.096899e+03   
    27    0.008212 2015-01-28  0.008212  0.007770  0.008075  8.608624e+02   
    28    0.008250 2015-01-29  0.008489  0.008046  0.008050  1.128248e+03   
    29    0.008380 2015-01-30  0.008438  0.008171  0.008250  3.743013e+02   
    ...        ...        ...       ...       ...       ...           ...   
    1059  0.010101 2017-11-25  0.010230  0.009190  0.009468  3.031930e+05   
    1060  0.009180 2017-11-26  0.010135  0.008910  0.010121  2.541441e+05   
    1061  0.009311 2017-11-27  0.009646  0.008900  0.009180  1.717017e+05   
    1062  0.009525 2017-11-28  0.009600  0.009101  0.009311  1.850281e+05   
    1063  0.008553 2017-11-29  0.009996  0.008428  0.009525  3.711763e+05   
    1064  0.008669 2017-11-30  0.008884  0.008320  0.008566  1.653688e+05   
    1065  0.008983 2017-12-01  0.009250  0.008382  0.008666  1.480540e+05   
    1066  0.009067 2017-12-02  0.009401  0.008900  0.009020  1.011444e+05   
    1067  0.008939 2017-12-03  0.009173  0.008560  0.009080  1.017863e+05   
    1068  0.008958 2017-12-04  0.008993  0.008620  0.008974  7.213935e+04   
    1069  0.008593 2017-12-05  0.008943  0.008500  0.008925  9.282131e+04   
    1070  0.007064 2017-12-06  0.008682  0.006726  0.008593  3.124364e+05   
    1071  0.005629 2017-12-07  0.007186  0.005348  0.007130  4.754949e+05   
    1072  0.007580 2017-12-08  0.008600  0.005321  0.005619  7.168409e+05   
    1073  0.010160 2017-12-09  0.010451  0.007499  0.007596  9.129953e+05   
    1074  0.009676 2017-12-10  0.010285  0.009041  0.010160  3.329893e+05   
    1075  0.012520 2017-12-11  0.013499  0.009055  0.009634  8.057766e+05   
    1076  0.017421 2017-12-12  0.019287  0.012274  0.012586  1.445482e+06   
    1077  0.018274 2017-12-13  0.019950  0.017032  0.017320  5.133211e+05   
    1078  0.016790 2017-12-14  0.018882  0.015800  0.018216  3.953086e+05   
    1079  0.016910 2017-12-15  0.018040  0.013828  0.016780  4.508953e+05   
    1080  0.015390 2017-12-16  0.017421  0.014860  0.016910  1.949037e+05   
    1081  0.016565 2017-12-17  0.017255  0.015304  0.015390  1.806767e+05   
    1082  0.018750 2017-12-18  0.019396  0.016350  0.016560  2.097592e+05   
    1083  0.019633 2017-12-19  0.020048  0.018300  0.018750  2.375660e+05   
    1084  0.018536 2017-12-20  0.019750  0.018319  0.019700  3.048701e+05   
    1085  0.019558 2017-12-21  0.019789  0.018322  0.018500  2.381299e+05   
    1086  0.018840 2017-12-22  0.019912  0.016386  0.019605  3.975168e+05   
    1087  0.019250 2017-12-23  0.019600  0.018531  0.018840  1.120855e+05   
    1088  0.019325 2017-12-24  0.020000  0.018889  0.019200  8.028097e+04   
    
                volume  weightedAverage  
    0         1.655467         0.008559  
    1         2.879419         0.008504  
    2        13.117114         0.007926  
    3         8.682349         0.007515  
    4         5.746375         0.007660  
    5         3.155705         0.007627  
    6         3.970395         0.007438  
    7         6.815195         0.007232  
    8         4.184491         0.007030  
    9         4.810775         0.006379  
    10       13.209860         0.006553  
    11        4.440818         0.006533  
    12        8.060932         0.006619  
    13       10.836602         0.006965  
    14        7.997775         0.006463  
    15        6.845352         0.006765  
    16       17.112279         0.006769  
    17        3.223630         0.006401  
    18        7.938914         0.006320  
    19        7.192207         0.006407  
    20        2.767880         0.006285  
    21        3.193189         0.006190  
    22        3.137080         0.006202  
    23       16.372795         0.007238  
    24       39.850341         0.008752  
    25       44.195346         0.008092  
    26        8.711298         0.007942  
    27        6.907032         0.008023  
    28        9.322035         0.008262  
    29        3.105789         0.008298  
    ...            ...              ...  
    1059   2935.077850         0.009681  
    1060   2407.783712         0.009474  
    1061   1597.514915         0.009304  
    1062   1732.532837         0.009364  
    1063   3407.472045         0.009180  
    1064   1417.036995         0.008569  
    1065   1301.122276         0.008788  
    1066    922.094896         0.009117  
    1067    907.923576         0.008920  
    1068    632.520007         0.008768  
    1069    804.161438         0.008664  
    1070   2374.576716         0.007600  
    1071   2944.878024         0.006193  
    1072   5249.691836         0.007323  
    1073   8722.764253         0.009554  
    1074   3238.381945         0.009725  
    1075   9120.373708         0.011319  
    1076  23407.281216         0.016193  
    1077   9411.224329         0.018334  
    1078   6869.075314         0.017376  
    1079   7216.175332         0.016004  
    1080   3137.432431         0.016097  
    1081   2970.967721         0.016444  
    1082   3698.146624         0.017630  
    1083   4574.632329         0.019256  
    1084   5782.110458         0.018966  
    1085   4521.955559         0.018989  
    1086   7309.445720         0.018388  
    1087   2152.105142         0.019201  
    1088   1552.748315         0.019341  
    
    [1089 rows x 8 columns],          close       date      high       low      open   quoteVolume  \
    0     0.000076 2015-01-01  0.000077  0.000075  0.000076  8.533084e+04   
    1     0.000078 2015-01-02  0.000078  0.000076  0.000077  2.355432e+05   
    2     0.000074 2015-01-03  0.000078  0.000069  0.000078  6.786861e+05   
    3     0.000069 2015-01-04  0.000074  0.000066  0.000072  1.375164e+06   
    4     0.000075 2015-01-05  0.000076  0.000068  0.000068  5.179275e+05   
    5     0.000073 2015-01-06  0.000076  0.000072  0.000074  7.620949e+05   
    6     0.000072 2015-01-07  0.000075  0.000070  0.000073  6.379464e+05   
    7     0.000072 2015-01-08  0.000073  0.000071  0.000072  5.521591e+05   
    8     0.000072 2015-01-09  0.000074  0.000072  0.000073  3.073595e+05   
    9     0.000071 2015-01-10  0.000074  0.000069  0.000072  9.566906e+05   
    10    0.000071 2015-01-11  0.000071  0.000066  0.000071  1.322687e+06   
    11    0.000066 2015-01-12  0.000071  0.000066  0.000071  7.132228e+05   
    12    0.000065 2015-01-13  0.000069  0.000061  0.000068  6.258480e+05   
    13    0.000080 2015-01-14  0.000082  0.000064  0.000065  1.751876e+06   
    14    0.000080 2015-01-15  0.000086  0.000076  0.000082  1.454204e+06   
    15    0.000078 2015-01-16  0.000081  0.000076  0.000080  4.266504e+05   
    16    0.000076 2015-01-17  0.000078  0.000076  0.000077  1.063233e+05   
    17    0.000075 2015-01-18  0.000077  0.000072  0.000076  3.054242e+05   
    18    0.000076 2015-01-19  0.000077  0.000074  0.000075  1.680644e+05   
    19    0.000079 2015-01-20  0.000079  0.000075  0.000076  7.297387e+05   
    20    0.000075 2015-01-21  0.000081  0.000075  0.000079  5.460306e+05   
    21    0.000073 2015-01-22  0.000075  0.000072  0.000075  5.414719e+05   
    22    0.000071 2015-01-23  0.000074  0.000071  0.000073  3.247890e+05   
    23    0.000067 2015-01-24  0.000073  0.000065  0.000071  8.481677e+05   
    24    0.000065 2015-01-25  0.000070  0.000065  0.000067  5.315128e+05   
    25    0.000060 2015-01-26  0.000065  0.000053  0.000065  1.204968e+06   
    26    0.000059 2015-01-27  0.000064  0.000058  0.000059  4.997505e+05   
    27    0.000062 2015-01-28  0.000063  0.000059  0.000059  3.103278e+05   
    28    0.000061 2015-01-29  0.000066  0.000060  0.000062  3.713559e+05   
    29    0.000063 2015-01-30  0.000063  0.000061  0.000061  2.352259e+05   
    ...        ...        ...       ...       ...       ...           ...   
    1059  0.000029 2017-11-25  0.000030  0.000029  0.000030  4.384057e+07   
    1060  0.000026 2017-11-26  0.000029  0.000026  0.000029  4.014146e+07   
    1061  0.000025 2017-11-27  0.000027  0.000025  0.000026  6.099423e+07   
    1062  0.000028 2017-11-28  0.000029  0.000025  0.000025  1.169076e+08   
    1063  0.000024 2017-11-29  0.000028  0.000024  0.000028  1.495675e+08   
    1064  0.000024 2017-11-30  0.000025  0.000023  0.000024  6.780995e+07   
    1065  0.000023 2017-12-01  0.000024  0.000023  0.000024  4.164615e+07   
    1066  0.000022 2017-12-02  0.000023  0.000022  0.000023  2.666747e+07   
    1067  0.000022 2017-12-03  0.000023  0.000021  0.000022  4.543784e+07   
    1068  0.000021 2017-12-04  0.000022  0.000021  0.000022  3.554573e+07   
    1069  0.000020 2017-12-05  0.000021  0.000020  0.000021  8.907668e+07   
    1070  0.000016 2017-12-06  0.000020  0.000015  0.000020  1.747119e+08   
    1071  0.000012 2017-12-07  0.000016  0.000012  0.000016  2.096823e+08   
    1072  0.000015 2017-12-08  0.000017  0.000012  0.000012  3.702402e+08   
    1073  0.000016 2017-12-09  0.000018  0.000014  0.000015  1.261057e+08   
    1074  0.000015 2017-12-10  0.000018  0.000015  0.000016  1.215115e+08   
    1075  0.000015 2017-12-11  0.000015  0.000014  0.000015  8.496965e+07   
    1076  0.000022 2017-12-12  0.000026  0.000015  0.000015  4.622929e+08   
    1077  0.000028 2017-12-13  0.000031  0.000020  0.000022  5.619751e+08   
    1078  0.000048 2017-12-14  0.000052  0.000028  0.000028  8.655033e+08   
    1079  0.000042 2017-12-15  0.000050  0.000034  0.000048  5.387053e+08   
    1080  0.000038 2017-12-16  0.000046  0.000037  0.000042  2.115371e+08   
    1081  0.000037 2017-12-17  0.000040  0.000036  0.000038  1.082551e+08   
    1082  0.000040 2017-12-18  0.000043  0.000037  0.000037  1.658124e+08   
    1083  0.000042 2017-12-19  0.000045  0.000039  0.000040  1.804097e+08   
    1084  0.000044 2017-12-20  0.000045  0.000039  0.000042  1.316311e+08   
    1085  0.000073 2017-12-21  0.000074  0.000043  0.000044  4.549460e+08   
    1086  0.000075 2017-12-22  0.000086  0.000060  0.000073  4.954679e+08   
    1087  0.000071 2017-12-23  0.000078  0.000068  0.000075  8.859007e+07   
    1088  0.000070 2017-12-24  0.000072  0.000068  0.000071  4.028837e+07   
    
                volume  weightedAverage  
    0         6.468048         0.000076  
    1        18.283504         0.000078  
    2        50.189736         0.000074  
    3        94.421953         0.000069  
    4        38.303674         0.000074  
    5        56.548566         0.000074  
    6        45.547333         0.000071  
    7        39.691006         0.000072  
    8        22.495151         0.000073  
    9        68.052062         0.000071  
    10       90.746948         0.000069  
    11       49.030102         0.000069  
    12       40.870565         0.000065  
    13      129.080567         0.000074  
    14      116.541005         0.000080  
    15       33.434040         0.000078  
    16        8.192741         0.000077  
    17       22.790388         0.000075  
    18       12.620512         0.000075  
    19       56.011761         0.000077  
    20       42.253796         0.000077  
    21       39.455510         0.000073  
    22       23.376656         0.000072  
    23       58.721396         0.000069  
    24       35.152975         0.000066  
    25       69.389066         0.000058  
    26       30.575964         0.000061  
    27       18.860713         0.000061  
    28       23.580089         0.000063  
    29       14.608991         0.000062  
    ...            ...              ...  
    1059   1288.705937         0.000029  
    1060   1102.440315         0.000027  
    1061   1577.532195         0.000026  
    1062   3127.435356         0.000027  
    1063   3755.852081         0.000025  
    1064   1613.327334         0.000024  
    1065    977.550135         0.000023  
    1066    600.188656         0.000023  
    1067    995.022518         0.000022  
    1068    766.110402         0.000022  
    1069   1827.241622         0.000021  
    1070   3101.086334         0.000018  
    1071   2973.271549         0.000014  
    1072   5402.331838         0.000015  
    1073   2041.514837         0.000016  
    1074   1958.345201         0.000016  
    1075   1247.158274         0.000015  
    1076   9064.864869         0.000020  
    1077  14699.885749         0.000026  
    1078  34590.751245         0.000040  
    1079  22802.005222         0.000042  
    1080   8720.703689         0.000041  
    1081   4082.431780         0.000038  
    1082   6489.653375         0.000039  
    1083   7636.301970         0.000042  
    1084   5555.557937         0.000042  
    1085  26920.168335         0.000059  
    1086  36198.914075         0.000073  
    1087   6349.827513         0.000072  
    1088   2803.277212         0.000070  
    
    [1089 rows x 8 columns],         close       date      high       low      open   quoteVolume  \
    0    0.001410 2016-07-24  0.010000  0.000101  0.009950  1.734943e+07   
    1    0.000925 2016-07-25  0.001420  0.000669  0.001410  1.365341e+07   
    2    0.003310 2016-07-26  0.004872  0.000916  0.000925  5.199938e+07   
    3    0.002426 2016-07-27  0.003800  0.001860  0.003300  2.558753e+07   
    4    0.002320 2016-07-28  0.002500  0.002068  0.002440  3.118785e+06   
    5    0.002484 2016-07-29  0.002790  0.002355  0.002632  7.407740e+06   
    6    0.002396 2016-07-30  0.002650  0.002362  0.002484  3.869384e+06   
    7    0.002879 2016-07-31  0.003050  0.002390  0.002390  1.002239e+07   
    8    0.003888 2016-08-01  0.003890  0.002837  0.002875  1.290429e+07   
    9    0.005101 2016-08-02  0.006380  0.003741  0.003888  3.523149e+07   
    10   0.004580 2016-08-03  0.005820  0.004110  0.005101  1.971554e+07   
    11   0.004070 2016-08-04  0.004855  0.003611  0.004599  1.279000e+07   
    12   0.004480 2016-08-05  0.004600  0.003910  0.004069  5.847340e+06   
    13   0.004590 2016-08-06  0.005072  0.004360  0.004480  5.814864e+06   
    14   0.003638 2016-08-07  0.004670  0.003350  0.004590  1.007790e+07   
    15   0.003705 2016-08-08  0.004070  0.003398  0.003632  4.912277e+06   
    16   0.003305 2016-08-09  0.003799  0.003050  0.003705  7.680514e+06   
    17   0.002900 2016-08-10  0.003495  0.002736  0.003310  6.252950e+06   
    18   0.003136 2016-08-11  0.003339  0.002660  0.002900  6.251438e+06   
    19   0.003147 2016-08-12  0.003590  0.002890  0.003136  5.402648e+06   
    20   0.003195 2016-08-13  0.003310  0.003100  0.003156  1.359306e+06   
    21   0.003379 2016-08-14  0.003500  0.003188  0.003195  2.314282e+06   
    22   0.003283 2016-08-15  0.003515  0.003230  0.003379  2.197941e+06   
    23   0.003218 2016-08-16  0.003340  0.003153  0.003282  1.207315e+06   
    24   0.003029 2016-08-17  0.003250  0.003010  0.003220  1.626010e+06   
    25   0.002928 2016-08-18  0.003115  0.002850  0.003038  1.588264e+06   
    26   0.003100 2016-08-19  0.003147  0.002790  0.002928  2.116995e+06   
    27   0.002997 2016-08-20  0.003100  0.002879  0.003100  1.113022e+06   
    28   0.002973 2016-08-21  0.003063  0.002925  0.002997  6.356466e+05   
    29   0.002845 2016-08-22  0.003009  0.002751  0.002973  1.810849e+06   
    ..        ...        ...       ...       ...       ...           ...   
    489  0.002432 2017-11-25  0.002650  0.002412  0.002550  4.841694e+05   
    490  0.002338 2017-11-26  0.002455  0.002263  0.002436  2.416817e+05   
    491  0.002534 2017-11-27  0.002704  0.002235  0.002338  6.529619e+05   
    492  0.003181 2017-11-28  0.003371  0.002527  0.002534  1.480816e+06   
    493  0.002480 2017-11-29  0.003211  0.002444  0.003181  1.153999e+06   
    494  0.002606 2017-11-30  0.002763  0.002403  0.002494  5.713708e+05   
    495  0.002753 2017-12-01  0.002890  0.002512  0.002606  4.185788e+05   
    496  0.002659 2017-12-02  0.002799  0.002588  0.002737  2.474886e+05   
    497  0.002580 2017-12-03  0.002696  0.002547  0.002645  2.009439e+05   
    498  0.002522 2017-12-04  0.002588  0.002502  0.002582  1.348050e+05   
    499  0.002412 2017-12-05  0.002528  0.002380  0.002519  2.356702e+05   
    500  0.001822 2017-12-06  0.002414  0.001724  0.002414  5.030883e+05   
    501  0.001400 2017-12-07  0.001940  0.001380  0.001832  8.568841e+05   
    502  0.001746 2017-12-08  0.001798  0.001309  0.001394  1.131038e+06   
    503  0.001875 2017-12-09  0.002100  0.001677  0.001750  5.841926e+05   
    504  0.001704 2017-12-10  0.001927  0.001634  0.001876  5.030188e+05   
    505  0.001604 2017-12-11  0.001850  0.001500  0.001693  4.258664e+05   
    506  0.001743 2017-12-12  0.001939  0.001582  0.001611  8.819380e+05   
    507  0.001798 2017-12-13  0.001960  0.001670  0.001744  8.436060e+05   
    508  0.001916 2017-12-14  0.002094  0.001775  0.001798  7.156816e+05   
    509  0.001797 2017-12-15  0.001933  0.001533  0.001916  6.538346e+05   
    510  0.001750 2017-12-16  0.002172  0.001710  0.001793  1.051629e+06   
    511  0.001794 2017-12-17  0.001882  0.001710  0.001753  3.950995e+05   
    512  0.002007 2017-12-18  0.002200  0.001746  0.001797  1.006611e+06   
    513  0.002155 2017-12-19  0.002300  0.001995  0.002013  9.065073e+05   
    514  0.002375 2017-12-20  0.002467  0.002114  0.002167  6.622078e+05   
    515  0.002384 2017-12-21  0.002500  0.002310  0.002374  6.007119e+05   
    516  0.002062 2017-12-22  0.002396  0.001901  0.002384  9.175843e+05   
    517  0.002045 2017-12-23  0.002195  0.002025  0.002062  3.903120e+05   
    518  0.002011 2017-12-24  0.002079  0.001984  0.002035  2.152186e+05   
    
                volume  weightedAverage  
    0     24099.433042         0.001389  
    1     13184.156647         0.000966  
    2    144768.573933         0.002784  
    3     72354.908804         0.002828  
    4      7175.411011         0.002301  
    5     18818.364241         0.002540  
    6      9626.733679         0.002488  
    7     28118.650002         0.002806  
    8     43661.584760         0.003383  
    9    170119.710287         0.004829  
    10    98313.613451         0.004987  
    11    54190.860282         0.004237  
    12    25320.237021         0.004330  
    13    27219.710735         0.004681  
    14    39099.333632         0.003880  
    15    18256.610781         0.003717  
    16    25738.462916         0.003351  
    17    19191.401723         0.003069  
    18    19060.873877         0.003049  
    19    17885.623676         0.003311  
    20     4324.068125         0.003181  
    21     7759.864810         0.003353  
    22     7413.362634         0.003373  
    23     3898.788574         0.003229  
    24     5045.951881         0.003103  
    25     4720.046326         0.002972  
    26     6229.954594         0.002943  
    27     3317.906103         0.002981  
    28     1892.877824         0.002978  
    29     5174.972405         0.002858  
    ..             ...              ...  
    489    1230.221180         0.002541  
    490     571.221509         0.002364  
    491    1606.969104         0.002461  
    492    4529.281641         0.003059  
    493    3132.578131         0.002715  
    494    1454.504336         0.002546  
    495    1125.913331         0.002690  
    496     668.657705         0.002702  
    497     522.807276         0.002602  
    498     342.577527         0.002541  
    499     576.860890         0.002448  
    500    1050.664527         0.002088  
    501    1425.998850         0.001664  
    502    1796.794203         0.001589  
    503    1104.535052         0.001891  
    504     898.008856         0.001785  
    505     707.112384         0.001660  
    506    1525.127728         0.001729  
    507    1528.185814         0.001811  
    508    1362.676663         0.001904  
    509    1115.412974         0.001706  
    510    2051.847570         0.001951  
    511     703.013592         0.001779  
    512    2006.134189         0.001993  
    513    1963.066150         0.002166  
    514    1526.943945         0.002306  
    515    1446.296240         0.002408  
    516    1953.668289         0.002129  
    517     816.599430         0.002092  
    518     433.891472         0.002016  
    
    [519 rows x 8 columns],          close       date      high       low      open   quoteVolume  \
    0     0.000018 2015-01-01  0.000018  0.000017  0.000017  1.152676e+06   
    1     0.000018 2015-01-02  0.000018  0.000017  0.000017  3.886411e+06   
    2     0.000016 2015-01-03  0.000018  0.000016  0.000018  9.187188e+06   
    3     0.000017 2015-01-04  0.000017  0.000016  0.000016  7.835991e+06   
    4     0.000019 2015-01-05  0.000022  0.000016  0.000017  7.365508e+06   
    5     0.000017 2015-01-06  0.000019  0.000017  0.000019  4.806229e+06   
    6     0.000018 2015-01-07  0.000018  0.000017  0.000017  2.889559e+06   
    7     0.000020 2015-01-08  0.000020  0.000017  0.000018  9.244971e+06   
    8     0.000020 2015-01-09  0.000021  0.000019  0.000019  5.053338e+06   
    9     0.000019 2015-01-10  0.000020  0.000018  0.000020  6.573156e+06   
    10    0.000019 2015-01-11  0.000019  0.000018  0.000019  1.557399e+06   
    11    0.000018 2015-01-12  0.000019  0.000018  0.000019  3.115290e+06   
    12    0.000019 2015-01-13  0.000020  0.000018  0.000018  8.090623e+06   
    13    0.000026 2015-01-14  0.000026  0.000018  0.000019  1.416294e+07   
    14    0.000023 2015-01-15  0.000026  0.000022  0.000025  1.187758e+07   
    15    0.000021 2015-01-16  0.000024  0.000020  0.000024  7.648548e+06   
    16    0.000024 2015-01-17  0.000024  0.000020  0.000021  3.191257e+06   
    17    0.000023 2015-01-18  0.000024  0.000022  0.000024  4.922116e+06   
    18    0.000023 2015-01-19  0.000027  0.000022  0.000023  4.771113e+06   
    19    0.000022 2015-01-20  0.000024  0.000022  0.000023  2.695911e+06   
    20    0.000023 2015-01-21  0.000024  0.000022  0.000023  4.066342e+06   
    21    0.000021 2015-01-22  0.000024  0.000021  0.000023  4.121497e+06   
    22    0.000021 2015-01-23  0.000022  0.000020  0.000021  3.046104e+06   
    23    0.000020 2015-01-24  0.000022  0.000020  0.000021  6.992650e+06   
    24    0.000019 2015-01-25  0.000020  0.000019  0.000020  6.265726e+06   
    25    0.000016 2015-01-26  0.000019  0.000015  0.000019  1.353723e+07   
    26    0.000017 2015-01-27  0.000018  0.000016  0.000017  3.578731e+06   
    27    0.000017 2015-01-28  0.000017  0.000016  0.000017  2.643589e+06   
    28    0.000018 2015-01-29  0.000020  0.000017  0.000017  6.843740e+06   
    29    0.000018 2015-01-30  0.000019  0.000018  0.000018  1.628083e+06   
    ...        ...        ...       ...       ...       ...           ...   
    1059  0.000005 2017-11-25  0.000005  0.000005  0.000005  9.243755e+07   
    1060  0.000005 2017-11-26  0.000005  0.000005  0.000005  2.309649e+08   
    1061  0.000006 2017-11-27  0.000006  0.000005  0.000005  3.353144e+08   
    1062  0.000008 2017-11-28  0.000008  0.000006  0.000006  8.905726e+08   
    1063  0.000007 2017-11-29  0.000009  0.000006  0.000008  1.398073e+09   
    1064  0.000007 2017-11-30  0.000008  0.000007  0.000007  3.705586e+08   
    1065  0.000008 2017-12-01  0.000008  0.000007  0.000007  5.003988e+08   
    1066  0.000009 2017-12-02  0.000009  0.000008  0.000008  3.528554e+08   
    1067  0.000008 2017-12-03  0.000009  0.000008  0.000009  2.355436e+08   
    1068  0.000008 2017-12-04  0.000008  0.000008  0.000008  1.447755e+08   
    1069  0.000011 2017-12-05  0.000011  0.000008  0.000008  5.561217e+08   
    1070  0.000010 2017-12-06  0.000012  0.000009  0.000011  1.010079e+09   
    1071  0.000008 2017-12-07  0.000012  0.000007  0.000010  7.189752e+08   
    1072  0.000008 2017-12-08  0.000009  0.000007  0.000008  5.729846e+08   
    1073  0.000009 2017-12-09  0.000010  0.000008  0.000008  2.259655e+08   
    1074  0.000008 2017-12-10  0.000009  0.000007  0.000009  1.990196e+08   
    1075  0.000009 2017-12-11  0.000009  0.000008  0.000008  2.231381e+08   
    1076  0.000009 2017-12-12  0.000009  0.000008  0.000009  3.022317e+08   
    1077  0.000009 2017-12-13  0.000010  0.000008  0.000009  2.011951e+08   
    1078  0.000011 2017-12-14  0.000013  0.000009  0.000009  5.041819e+08   
    1079  0.000011 2017-12-15  0.000012  0.000009  0.000011  4.146102e+08   
    1080  0.000012 2017-12-16  0.000012  0.000010  0.000011  2.699601e+08   
    1081  0.000014 2017-12-17  0.000015  0.000012  0.000012  4.718192e+08   
    1082  0.000014 2017-12-18  0.000015  0.000014  0.000014  1.708836e+08   
    1083  0.000015 2017-12-19  0.000016  0.000014  0.000014  2.182587e+08   
    1084  0.000015 2017-12-20  0.000016  0.000012  0.000015  2.873117e+08   
    1085  0.000016 2017-12-21  0.000017  0.000015  0.000015  1.927778e+08   
    1086  0.000015 2017-12-22  0.000018  0.000013  0.000016  2.742088e+08   
    1087  0.000016 2017-12-23  0.000017  0.000015  0.000016  1.205555e+08   
    1088  0.000016 2017-12-24  0.000016  0.000015  0.000016  4.675464e+07   
    
                volume  weightedAverage  
    0        19.922657         0.000017  
    1        68.145004         0.000018  
    2       155.926477         0.000017  
    3       128.085828         0.000016  
    4       132.299232         0.000018  
    5        86.389194         0.000018  
    6        50.854583         0.000018  
    7       177.195174         0.000019  
    8       100.312762         0.000020  
    9       126.939280         0.000019  
    10       29.262164         0.000019  
    11       57.073134         0.000018  
    12      147.316451         0.000018  
    13      305.729133         0.000022  
    14      285.157508         0.000024  
    15      169.574408         0.000022  
    16       72.603901         0.000023  
    17      114.462227         0.000023  
    18      113.903245         0.000024  
    19       61.349057         0.000023  
    20       93.773905         0.000023  
    21       89.813325         0.000022  
    22       63.540983         0.000021  
    23      143.584376         0.000021  
    24      121.148271         0.000019  
    25      225.352211         0.000017  
    26       60.591468         0.000017  
    27       44.264071         0.000017  
    28      121.556886         0.000018  
    29       29.967962         0.000018  
    ...            ...              ...  
    1059    459.382165         0.000005  
    1060   1190.462635         0.000005  
    1061   1889.874914         0.000006  
    1062   6077.858275         0.000007  
    1063  11044.124694         0.000008  
    1064   2650.793686         0.000007  
    1065   3896.601538         0.000008  
    1066   3054.974494         0.000009  
    1067   1911.841407         0.000008  
    1068   1174.953182         0.000008  
    1069   5330.555505         0.000010  
    1070  11009.855536         0.000011  
    1071   6733.876201         0.000009  
    1072   4630.194130         0.000008  
    1073   2066.167580         0.000009  
    1074   1612.671705         0.000008  
    1075   1856.160640         0.000008  
    1076   2617.542672         0.000009  
    1077   1821.951019         0.000009  
    1078   5357.484449         0.000011  
    1079   4316.017269         0.000010  
    1080   3000.106878         0.000011  
    1081   6480.241590         0.000014  
    1082   2431.690228         0.000014  
    1083   3305.843155         0.000015  
    1084   4065.433213         0.000014  
    1085   3045.216746         0.000016  
    1086   4365.928659         0.000016  
    1087   1923.587491         0.000016  
    1088    732.575678         0.000016  
    
    [1089 rows x 8 columns],          close       date      high       low      open   quoteVolume  \
    0     0.006194 2015-01-01  0.006277  0.006002  0.006056     59.061391   
    1     0.006300 2015-01-02  0.006400  0.006049  0.006143    209.561843   
    2     0.006213 2015-01-03  0.006300  0.006000  0.006074   1067.683160   
    3     0.006200 2015-01-04  0.006301  0.006000  0.006000    353.164407   
    4     0.006000 2015-01-05  0.006200  0.006000  0.006110    453.520104   
    5     0.006000 2015-01-06  0.006200  0.006000  0.006000    306.535292   
    6     0.005890 2015-01-07  0.006053  0.005837  0.006000    437.290342   
    7     0.006051 2015-01-08  0.006056  0.005800  0.005890    253.036201   
    8     0.005998 2015-01-09  0.006120  0.005830  0.005853    136.042749   
    9     0.005938 2015-01-10  0.006222  0.005920  0.005996    222.843197   
    10    0.006057 2015-01-11  0.006222  0.005953  0.006216    635.047033   
    11    0.006132 2015-01-12  0.006220  0.005959  0.006057    336.665846   
    12    0.006000 2015-01-13  0.006400  0.006000  0.006131    560.988820   
    13    0.006098 2015-01-14  0.006180  0.005942  0.006005   1045.770887   
    14    0.006071 2015-01-15  0.006199  0.005982  0.006175    215.015186   
    15    0.006166 2015-01-16  0.006420  0.005878  0.006197   1035.890785   
    16    0.006120 2015-01-17  0.006245  0.005400  0.006164    761.265073   
    17    0.006158 2015-01-18  0.006250  0.006051  0.006123    250.304726   
    18    0.006330 2015-01-19  0.006397  0.006152  0.006152    302.540303   
    19    0.007104 2015-01-20  0.007104  0.006230  0.006360    840.839109   
    20    0.007018 2015-01-21  0.007490  0.006607  0.006607   1107.756457   
    21    0.006811 2015-01-22  0.007316  0.006780  0.007018    613.461425   
    22    0.006801 2015-01-23  0.007146  0.006762  0.006997    568.807191   
    23    0.006900 2015-01-24  0.007151  0.006674  0.006800    849.668985   
    24    0.007041 2015-01-25  0.007310  0.006900  0.006902   1496.963143   
    25    0.006800 2015-01-26  0.007246  0.006661  0.007246   1180.672875   
    26    0.006950 2015-01-27  0.007100  0.006581  0.006800    416.525556   
    27    0.006839 2015-01-28  0.007050  0.006717  0.006800    209.251456   
    28    0.007236 2015-01-29  0.007251  0.006352  0.007037    660.710582   
    29    0.007250 2015-01-30  0.007251  0.006352  0.007162    936.299831   
    ...        ...        ...       ...       ...       ...           ...   
    1059  0.072520 2017-11-25  0.074992  0.068400  0.068400  30287.380230   
    1060  0.066624 2017-11-26  0.072965  0.064800  0.072705  22173.880965   
    1061  0.064033 2017-11-27  0.066722  0.063500  0.066624  14722.277886   
    1062  0.061603 2017-11-28  0.065150  0.060807  0.064031  16566.647061   
    1063  0.068000 2017-11-29  0.072500  0.057806  0.061603  60736.430650   
    1064  0.076590 2017-11-30  0.080300  0.065715  0.068000  62651.496667   
    1065  0.072199 2017-12-01  0.079506  0.071300  0.076185  32443.384727   
    1066  0.070210 2017-12-02  0.072545  0.068475  0.072199  11790.020931   
    1067  0.067748 2017-12-03  0.070496  0.065910  0.070210   9723.325427   
    1068  0.066130 2017-12-04  0.068242  0.066000  0.067748   8172.831110   
    1069  0.063296 2017-12-05  0.066600  0.062750  0.066130  14702.077615   
    1070  0.048710 2017-12-06  0.064500  0.048000  0.063200  28689.668441   
    1071  0.039510 2017-12-07  0.050661  0.039182  0.048720  41112.473329   
    1072  0.044000 2017-12-08  0.053310  0.036227  0.039510  44980.451384   
    1073  0.047953 2017-12-09  0.052988  0.043348  0.044000  22524.793576   
    1074  0.045257 2017-12-10  0.050618  0.043697  0.047932  16423.516104   
    1075  0.045500 2017-12-11  0.046613  0.042164  0.045257  13051.203058   
    1076  0.054600 2017-12-12  0.055970  0.043945  0.045406  32382.986444   
    1077  0.054088 2017-12-13  0.055220  0.048896  0.054417  22541.756077   
    1078  0.056751 2017-12-14  0.059511  0.052063  0.054240  26163.044035   
    1079  0.050874 2017-12-15  0.057076  0.046500  0.056751  19452.731346   
    1080  0.051430 2017-12-16  0.052750  0.046520  0.050874  11598.466091   
    1081  0.057000 2017-12-17  0.060000  0.051430  0.051713  29292.902955   
    1082  0.060215 2017-12-18  0.063621  0.055023  0.057155  16695.872644   
    1083  0.065491 2017-12-19  0.066644  0.060300  0.060570  22765.881017   
    1084  0.086915 2017-12-20  0.090110  0.065290  0.065410  55060.341225   
    1085  0.086746 2017-12-21  0.091996  0.083800  0.086901  25389.935169   
    1086  0.082091 2017-12-22  0.089999  0.072750  0.086746  37440.812594   
    1087  0.083285 2017-12-23  0.085384  0.080287  0.082091  14252.691853   
    1088  0.081013 2017-12-24  0.083284  0.080519  0.082600   5378.506522   
    
               volume  weightedAverage  
    0        0.368924         0.006246  
    1        1.310185         0.006252  
    2        6.438556         0.006030  
    3        2.176665         0.006163  
    4        2.760890         0.006088  
    5        1.863959         0.006081  
    6        2.595334         0.005935  
    7        1.507623         0.005958  
    8        0.814757         0.005989  
    9        1.349934         0.006058  
    10       3.856036         0.006072  
    11       2.050108         0.006089  
    12       3.413299         0.006084  
    13       6.302037         0.006026  
    14       1.313792         0.006110  
    15       6.420017         0.006198  
    16       4.460214         0.005859  
    17       1.552661         0.006203  
    18       1.897638         0.006272  
    19       5.557597         0.006610  
    20       7.910255         0.007141  
    21       4.310446         0.007026  
    22       3.973206         0.006985  
    23       5.905019         0.006950  
    24      10.646401         0.007112  
    25       8.110318         0.006869  
    26       2.833755         0.006803  
    27       1.447473         0.006917  
    28       4.590579         0.006948  
    29       6.435743         0.006874  
    ...           ...              ...  
    1059  2183.043498         0.072078  
    1060  1522.140043         0.068646  
    1061   957.801010         0.065058  
    1062  1037.761623         0.062642  
    1063  3997.054757         0.065810  
    1064  4681.846510         0.074728  
    1065  2429.371477         0.074880  
    1066   827.553459         0.070191  
    1067   663.803377         0.068269  
    1068   544.111483         0.066576  
    1069   948.132334         0.064490  
    1070  1598.383865         0.055713  
    1071  1827.745271         0.044457  
    1072  2014.111118         0.044777  
    1073  1105.324663         0.049071  
    1074   770.702887         0.046927  
    1075   573.455982         0.043939  
    1076  1616.110051         0.049906  
    1077  1192.197450         0.052888  
    1078  1466.103559         0.056037  
    1079   976.386545         0.050193  
    1080   576.393948         0.049696  
    1081  1644.489912         0.056140  
    1082   977.218606         0.058531  
    1083  1460.689775         0.064161  
    1084  4461.969968         0.081038  
    1085  2234.075048         0.087991  
    1086  3024.315818         0.080776  
    1087  1180.203656         0.082806  
    1088   439.376732         0.081691  
    
    [1089 rows x 8 columns],             close       date          high           low          open  \
    0    2.400000e-07 2015-08-25  6.900000e-05  1.500000e-07  5.000000e-07   
    1    1.800000e-07 2015-08-26  3.300000e-07  1.400000e-07  2.500000e-07   
    2    1.900000e-07 2015-08-27  2.400000e-07  1.700000e-07  1.800000e-07   
    3    1.700000e-07 2015-08-28  2.100000e-07  1.500000e-07  1.900000e-07   
    4    1.500000e-07 2015-08-29  1.800000e-07  1.500000e-07  1.700000e-07   
    5    1.400000e-07 2015-08-30  1.700000e-07  1.200000e-07  1.600000e-07   
    6    1.300000e-07 2015-08-31  1.500000e-07  1.200000e-07  1.300000e-07   
    7    1.300000e-07 2015-09-01  1.400000e-07  1.300000e-07  1.400000e-07   
    8    1.200000e-07 2015-09-02  1.400000e-07  1.200000e-07  1.200000e-07   
    9    1.100000e-07 2015-09-03  1.200000e-07  1.000000e-07  1.200000e-07   
    10   1.100000e-07 2015-09-04  1.100000e-07  1.000000e-07  1.100000e-07   
    11   1.000000e-07 2015-09-05  1.100000e-07  1.000000e-07  1.100000e-07   
    12   1.200000e-07 2015-09-06  1.200000e-07  1.000000e-07  1.100000e-07   
    13   1.800000e-07 2015-09-07  2.100000e-07  1.100000e-07  1.100000e-07   
    14   3.100000e-07 2015-09-08  3.800000e-07  1.700000e-07  1.800000e-07   
    15   2.000000e-07 2015-09-09  3.200000e-07  1.900000e-07  3.100000e-07   
    16   1.800000e-07 2015-09-10  2.300000e-07  1.600000e-07  2.000000e-07   
    17   1.800000e-07 2015-09-11  2.000000e-07  1.600000e-07  2.000000e-07   
    18   1.400000e-07 2015-09-12  1.900000e-07  1.300000e-07  1.800000e-07   
    19   1.500000e-07 2015-09-13  1.700000e-07  1.300000e-07  1.500000e-07   
    20   1.500000e-07 2015-09-14  1.700000e-07  1.500000e-07  1.600000e-07   
    21   1.500000e-07 2015-09-15  1.600000e-07  1.300000e-07  1.400000e-07   
    22   1.300000e-07 2015-09-16  1.500000e-07  1.300000e-07  1.500000e-07   
    23   1.300000e-07 2015-09-17  1.400000e-07  1.200000e-07  1.300000e-07   
    24   1.300000e-07 2015-09-18  1.400000e-07  1.200000e-07  1.300000e-07   
    25   1.200000e-07 2015-09-19  1.300000e-07  1.100000e-07  1.300000e-07   
    26   1.200000e-07 2015-09-20  1.400000e-07  1.200000e-07  1.300000e-07   
    27   1.300000e-07 2015-09-21  1.400000e-07  1.200000e-07  1.300000e-07   
    28   1.400000e-07 2015-09-22  1.400000e-07  1.300000e-07  1.400000e-07   
    29   1.400000e-07 2015-09-23  1.500000e-07  1.300000e-07  1.400000e-07   
    ..            ...        ...           ...           ...           ...   
    823  6.200000e-07 2017-11-25  6.400000e-07  5.900000e-07  6.100000e-07   
    824  6.000000e-07 2017-11-26  6.200000e-07  5.700000e-07  6.100000e-07   
    825  6.500000e-07 2017-11-27  6.600000e-07  5.800000e-07  5.900000e-07   
    826  6.400000e-07 2017-11-28  6.700000e-07  6.000000e-07  6.500000e-07   
    827  5.600000e-07 2017-11-29  6.600000e-07  5.400000e-07  6.400000e-07   
    828  5.500000e-07 2017-11-30  5.700000e-07  5.400000e-07  5.500000e-07   
    829  5.500000e-07 2017-12-01  5.800000e-07  5.400000e-07  5.400000e-07   
    830  6.300000e-07 2017-12-02  6.300000e-07  5.200000e-07  5.500000e-07   
    831  7.600000e-07 2017-12-03  8.200000e-07  6.000000e-07  6.300000e-07   
    832  9.300000e-07 2017-12-04  1.020000e-06  7.200000e-07  7.700000e-07   
    833  8.100000e-07 2017-12-05  9.400000e-07  7.500000e-07  9.300000e-07   
    834  6.200000e-07 2017-12-06  8.100000e-07  5.900000e-07  8.000000e-07   
    835  4.800000e-07 2017-12-07  6.600000e-07  4.700000e-07  6.200000e-07   
    836  5.300000e-07 2017-12-08  5.700000e-07  4.300000e-07  4.700000e-07   
    837  5.500000e-07 2017-12-09  5.900000e-07  5.100000e-07  5.200000e-07   
    838  5.000000e-07 2017-12-10  5.600000e-07  4.900000e-07  5.400000e-07   
    839  5.100000e-07 2017-12-11  5.300000e-07  4.900000e-07  5.000000e-07   
    840  6.200000e-07 2017-12-12  6.300000e-07  5.100000e-07  5.100000e-07   
    841  6.200000e-07 2017-12-13  6.800000e-07  5.600000e-07  6.100000e-07   
    842  7.000000e-07 2017-12-14  7.100000e-07  5.900000e-07  6.200000e-07   
    843  5.900000e-07 2017-12-15  7.000000e-07  5.600000e-07  6.900000e-07   
    844  6.500000e-07 2017-12-16  6.900000e-07  5.800000e-07  6.000000e-07   
    845  8.200000e-07 2017-12-17  9.300000e-07  6.500000e-07  6.600000e-07   
    846  8.300000e-07 2017-12-18  8.800000e-07  7.300000e-07  8.100000e-07   
    847  9.000000e-07 2017-12-19  9.200000e-07  7.700000e-07  8.300000e-07   
    848  1.340000e-06 2017-12-20  1.390000e-06  8.000000e-07  9.000000e-07   
    849  1.470000e-06 2017-12-21  1.680000e-06  1.190000e-06  1.330000e-06   
    850  1.400000e-06 2017-12-22  1.550000e-06  1.100000e-06  1.470000e-06   
    851  1.870000e-06 2017-12-23  2.000000e-06  1.380000e-06  1.400000e-06   
    852  2.060000e-06 2017-12-24  2.180000e-06  1.710000e-06  1.860000e-06   
    
          quoteVolume       volume  weightedAverage  
    0    1.796256e+08    38.440339     2.100000e-07  
    1    5.201470e+08   113.269147     2.100000e-07  
    2    2.073278e+08    42.733965     2.000000e-07  
    3    1.540448e+08    26.643325     1.700000e-07  
    4    5.399869e+07     8.812046     1.600000e-07  
    5    1.900786e+08    25.769900     1.300000e-07  
    6    7.498757e+07     9.774835     1.300000e-07  
    7    4.705769e+07     6.295358     1.300000e-07  
    8    6.990474e+07     8.744993     1.200000e-07  
    9    1.240728e+08    13.118071     1.000000e-07  
    10   1.588842e+07     1.697119     1.000000e-07  
    11   4.702391e+07     4.908807     1.000000e-07  
    12   5.894470e+07     6.175847     1.000000e-07  
    13   3.374210e+08    53.449774     1.500000e-07  
    14   1.427096e+09   391.542059     2.700000e-07  
    15   3.115432e+08    76.202088     2.400000e-07  
    16   3.022203e+08    56.657607     1.800000e-07  
    17   1.563567e+08    27.466438     1.700000e-07  
    18   1.341179e+08    20.731423     1.500000e-07  
    19   5.149747e+07     7.695969     1.400000e-07  
    20   7.099642e+07    11.133822     1.500000e-07  
    21   1.398324e+08    19.482952     1.300000e-07  
    22   5.838404e+07     8.095340     1.300000e-07  
    23   1.439084e+08    18.871249     1.300000e-07  
    24   3.587543e+07     4.481930     1.200000e-07  
    25   7.647422e+07     8.787550     1.100000e-07  
    26   9.533548e+07    12.100374     1.200000e-07  
    27   9.089458e+07    11.402053     1.200000e-07  
    28   3.533092e+07     4.713822     1.300000e-07  
    29   9.506643e+07    12.974036     1.300000e-07  
    ..            ...          ...              ...  
    823  1.770892e+08   108.429872     6.100000e-07  
    824  1.500822e+08    88.391652     5.800000e-07  
    825  2.008433e+08   125.338001     6.200000e-07  
    826  2.805649e+08   178.305727     6.300000e-07  
    827  3.338192e+08   199.977575     5.900000e-07  
    828  1.178402e+08    65.403494     5.500000e-07  
    829  1.319913e+08    73.559239     5.500000e-07  
    830  2.860307e+08   165.032086     5.700000e-07  
    831  1.288349e+09   928.917327     7.200000e-07  
    832  1.991602e+09  1808.286799     9.000000e-07  
    833  7.900678e+08   653.830971     8.200000e-07  
    834  7.980434e+08   570.175885     7.100000e-07  
    835  6.940610e+08   387.694970     5.500000e-07  
    836  5.532667e+08   281.422908     5.000000e-07  
    837  2.910884e+08   162.855378     5.500000e-07  
    838  2.983118e+08   156.042013     5.200000e-07  
    839  2.669750e+08   136.691084     5.100000e-07  
    840  4.771657e+08   262.601431     5.500000e-07  
    841  6.480685e+08   401.285426     6.100000e-07  
    842  7.089672e+08   455.483274     6.400000e-07  
    843  4.787528e+08   287.882385     6.000000e-07  
    844  5.618818e+08   350.661780     6.200000e-07  
    845  1.416722e+09  1133.867683     8.000000e-07  
    846  5.170679e+08   416.016086     8.000000e-07  
    847  5.589962e+08   479.890544     8.500000e-07  
    848  1.465182e+09  1665.364395     1.130000e-06  
    849  1.093328e+09  1556.225930     1.420000e-06  
    850  7.568315e+08   974.769349     1.280000e-06  
    851  1.187890e+09  2079.298449     1.750000e-06  
    852  6.575296e+08  1301.375736     1.970000e-06  
    
    [853 rows x 8 columns],          close       date      high       low      open    quoteVolume  \
    0     0.001549 2015-01-01  0.001579  0.001311  0.001332   42500.334142   
    1     0.001430 2015-01-02  0.001549  0.001370  0.001549   49065.143430   
    2     0.001660 2015-01-03  0.001691  0.001412  0.001427   69499.330582   
    3     0.001556 2015-01-04  0.001650  0.001452  0.001640   49664.577459   
    4     0.001540 2015-01-05  0.001689  0.001440  0.001546   60422.799660   
    5     0.001588 2015-01-06  0.001689  0.001490  0.001540   50438.349360   
    6     0.001579 2015-01-07  0.001620  0.001550  0.001589   23775.770806   
    7     0.001521 2015-01-08  0.001580  0.001510  0.001565   22570.134045   
    8     0.001414 2015-01-09  0.001558  0.001403  0.001521   22721.530876   
    9     0.001527 2015-01-10  0.001530  0.001337  0.001403   37687.251692   
    10    0.001418 2015-01-11  0.001527  0.001398  0.001527   14487.719221   
    11    0.001354 2015-01-12  0.001448  0.001337  0.001432   14942.207289   
    12    0.001348 2015-01-13  0.001391  0.001315  0.001349   33049.265584   
    13    0.001248 2015-01-14  0.001430  0.001230  0.001343   71670.896575   
    14    0.001248 2015-01-15  0.001261  0.001173  0.001243   32883.787363   
    15    0.001350 2015-01-16  0.001369  0.001180  0.001244   31462.428237   
    16    0.001233 2015-01-17  0.001350  0.001183  0.001337   30151.377633   
    17    0.001222 2015-01-18  0.001249  0.001121  0.001228   27076.059796   
    18    0.001188 2015-01-19  0.001245  0.001100  0.001222   50438.742302   
    19    0.001135 2015-01-20  0.001200  0.001115  0.001191   17298.538673   
    20    0.001145 2015-01-21  0.001149  0.001100  0.001129   25447.529219   
    21    0.001180 2015-01-22  0.001230  0.001120  0.001145   36531.739166   
    22    0.001218 2015-01-23  0.001241  0.001117  0.001181   26399.504217   
    23    0.001197 2015-01-24  0.001264  0.001140  0.001217   34261.822089   
    24    0.001230 2015-01-25  0.001350  0.001197  0.001197   57845.678020   
    25    0.001165 2015-01-26  0.001280  0.001116  0.001230   63319.252936   
    26    0.001229 2015-01-27  0.001300  0.001146  0.001164   43329.282689   
    27    0.001246 2015-01-28  0.001300  0.001190  0.001223   39906.877422   
    28    0.001316 2015-01-29  0.001376  0.001228  0.001236   76418.202489   
    29    0.001300 2015-01-30  0.001390  0.001263  0.001360   55309.622885   
    ...        ...        ...       ...       ...       ...            ...   
    1059  0.019130 2017-11-25  0.020100  0.019010  0.019300   44534.047737   
    1060  0.017500 2017-11-26  0.019293  0.017082  0.019122   59058.097312   
    1061  0.017780 2017-11-27  0.017780  0.016660  0.017490   52917.285961   
    1062  0.019825 2017-11-28  0.020000  0.017660  0.017665  128817.125061   
    1063  0.017117 2017-11-29  0.020093  0.016900  0.019890  140866.007928   
    1064  0.017525 2017-11-30  0.018728  0.016700  0.017256   90694.978717   
    1065  0.017421 2017-12-01  0.018123  0.017100  0.017614   50397.266799   
    1066  0.018358 2017-12-02  0.018500  0.016910  0.017421   39614.220482   
    1067  0.017490 2017-12-03  0.018817  0.017150  0.018358   63052.614542   
    1068  0.017958 2017-12-04  0.018000  0.017240  0.017490   30071.199948   
    1069  0.021100 2017-12-05  0.021250  0.017822  0.017958  148690.264432   
    1070  0.018600 2017-12-06  0.022500  0.018065  0.021100  259790.186194   
    1071  0.015420 2017-12-07  0.019700  0.015220  0.018500  194690.085964   
    1072  0.016568 2017-12-08  0.017900  0.014553  0.015430  117933.387086   
    1073  0.017304 2017-12-09  0.019040  0.016270  0.016568   73892.928136   
    1074  0.016069 2017-12-10  0.018100  0.015728  0.017380   58091.077107   
    1075  0.016532 2017-12-11  0.016579  0.015500  0.016037   63289.360081   
    1076  0.018000 2017-12-12  0.018618  0.016532  0.016532  107253.925589   
    1077  0.018536 2017-12-13  0.019880  0.017250  0.018000  107335.215923   
    1078  0.019595 2017-12-14  0.020482  0.018350  0.018512  111804.379991   
    1079  0.017660 2017-12-15  0.019595  0.016500  0.019530   83015.490405   
    1080  0.016831 2017-12-16  0.018595  0.016350  0.017655   64094.932647   
    1081  0.018250 2017-12-17  0.018679  0.016690  0.016826   92671.367661   
    1082  0.019595 2017-12-18  0.019748  0.017807  0.018244   67659.805724   
    1083  0.020500 2017-12-19  0.021000  0.019348  0.019595   97210.134208   
    1084  0.028674 2017-12-20  0.028800  0.019688  0.020500  302745.534902   
    1085  0.026722 2017-12-21  0.028900  0.024683  0.028674  114225.320939   
    1086  0.023954 2017-12-22  0.026727  0.020713  0.026670  171614.153782   
    1087  0.024531 2017-12-23  0.025578  0.023600  0.023954   59307.875465   
    1088  0.023857 2017-12-24  0.025499  0.023500  0.024390   36551.390585   
    
               volume  weightedAverage  
    0       60.273026         0.001418  
    1       70.850101         0.001444  
    2      109.248923         0.001572  
    3       76.239597         0.001535  
    4       92.246549         0.001527  
    5       80.999125         0.001606  
    6       37.487705         0.001577  
    7       34.788992         0.001541  
    8       33.396418         0.001470  
    9       53.842757         0.001429  
    10      20.629037         0.001424  
    11      20.686298         0.001384  
    12      44.412890         0.001344  
    13      92.959850         0.001297  
    14      39.587973         0.001204  
    15      39.922064         0.001269  
    16      37.695041         0.001250  
    17      32.014125         0.001182  
    18      58.628347         0.001162  
    19      19.603452         0.001133  
    20      28.528401         0.001121  
    21      42.768088         0.001171  
    22      30.896515         0.001170  
    23      41.547393         0.001213  
    24      72.301998         0.001250  
    25      74.588947         0.001178  
    26      52.497110         0.001212  
    27      49.273320         0.001235  
    28      99.675207         0.001304  
    29      71.496402         0.001293  
    ...           ...              ...  
    1059   866.442690         0.019456  
    1060  1057.514360         0.017906  
    1061   907.360551         0.017147  
    1062  2405.512869         0.018674  
    1063  2546.876684         0.018080  
    1064  1595.605321         0.017593  
    1065   885.635667         0.017573  
    1066   692.804351         0.017489  
    1067  1126.270086         0.017862  
    1068   528.024435         0.017559  
    1069  2914.461040         0.019601  
    1070  5480.293518         0.021095  
    1071  3308.839745         0.016995  
    1072  1920.429626         0.016284  
    1073  1310.371742         0.017733  
    1074   966.829405         0.016643  
    1075  1005.457684         0.015887  
    1076  1866.686961         0.017404  
    1077  1997.986593         0.018614  
    1078  2163.617544         0.019352  
    1079  1454.416770         0.017520  
    1080  1124.499758         0.017544  
    1081  1644.726636         0.017748  
    1082  1270.225266         0.018774  
    1083  1961.106678         0.020174  
    1084  7343.023935         0.024255  
    1085  3052.389959         0.026723  
    1086  4061.338798         0.023666  
    1087  1476.557716         0.024896  
    1088   884.626675         0.024202  
    
    [1089 rows x 8 columns],             close       date          high           low          open  \
    0    1.600000e-06 2015-03-31  1.020000e-04  7.100000e-07  2.500000e-06   
    1    9.800000e-07 2015-04-01  1.870000e-06  7.000000e-07  1.580000e-06   
    2    1.230000e-06 2015-04-02  1.300000e-06  9.100000e-07  1.000000e-06   
    3    1.220000e-06 2015-04-03  1.310000e-06  1.150000e-06  1.220000e-06   
    4    1.070000e-06 2015-04-04  1.250000e-06  9.900000e-07  1.240000e-06   
    5    8.900000e-07 2015-04-05  1.120000e-06  8.400000e-07  1.090000e-06   
    6    1.130000e-06 2015-04-06  1.160000e-06  8.800000e-07  8.900000e-07   
    7    9.600000e-07 2015-04-07  1.120000e-06  9.200000e-07  1.120000e-06   
    8    9.300000e-07 2015-04-08  1.020000e-06  9.100000e-07  9.700000e-07   
    9    9.200000e-07 2015-04-09  9.800000e-07  8.800000e-07  9.500000e-07   
    10   7.400000e-07 2015-04-10  9.400000e-07  7.100000e-07  9.200000e-07   
    11   6.900000e-07 2015-04-11  8.100000e-07  6.100000e-07  7.800000e-07   
    12   6.200000e-07 2015-04-12  7.300000e-07  6.000000e-07  7.100000e-07   
    13   6.200000e-07 2015-04-13  6.600000e-07  5.200000e-07  6.200000e-07   
    14   7.000000e-07 2015-04-14  8.300000e-07  6.100000e-07  6.200000e-07   
    15   6.900000e-07 2015-04-15  7.800000e-07  6.600000e-07  7.100000e-07   
    16   7.400000e-07 2015-04-16  7.700000e-07  6.800000e-07  6.900000e-07   
    17   7.500000e-07 2015-04-17  7.700000e-07  7.100000e-07  7.200000e-07   
    18   6.300000e-07 2015-04-18  7.600000e-07  6.200000e-07  7.600000e-07   
    19   6.600000e-07 2015-04-19  7.300000e-07  6.000000e-07  6.300000e-07   
    20   5.900000e-07 2015-04-20  6.700000e-07  5.600000e-07  6.700000e-07   
    21   5.800000e-07 2015-04-21  6.500000e-07  5.500000e-07  5.900000e-07   
    22   5.900000e-07 2015-04-22  6.300000e-07  5.500000e-07  5.800000e-07   
    23   5.800000e-07 2015-04-23  6.200000e-07  5.600000e-07  6.000000e-07   
    24   6.400000e-07 2015-04-24  6.400000e-07  5.700000e-07  5.800000e-07   
    25   6.200000e-07 2015-04-25  6.500000e-07  6.000000e-07  6.100000e-07   
    26   6.100000e-07 2015-04-26  6.200000e-07  5.800000e-07  6.200000e-07   
    27   7.000000e-07 2015-04-27  7.800000e-07  6.200000e-07  6.200000e-07   
    28   6.500000e-07 2015-04-28  7.100000e-07  6.400000e-07  6.900000e-07   
    29   6.500000e-07 2015-04-29  6.800000e-07  6.200000e-07  6.500000e-07   
    ..            ...        ...           ...           ...           ...   
    970  2.505000e-05 2017-11-25  2.599000e-05  2.475000e-05  2.538000e-05   
    971  2.289000e-05 2017-11-26  2.522000e-05  2.247000e-05  2.504000e-05   
    972  2.315000e-05 2017-11-27  2.320000e-05  2.181000e-05  2.291000e-05   
    973  2.472000e-05 2017-11-28  2.641000e-05  2.260000e-05  2.304000e-05   
    974  2.213000e-05 2017-11-29  2.531000e-05  2.163000e-05  2.475000e-05   
    975  2.220000e-05 2017-11-30  2.309000e-05  2.196000e-05  2.213000e-05   
    976  2.183000e-05 2017-12-01  2.277000e-05  2.162000e-05  2.220000e-05   
    977  2.305000e-05 2017-12-02  2.376000e-05  2.180000e-05  2.186000e-05   
    978  2.417000e-05 2017-12-03  2.580000e-05  2.268000e-05  2.304000e-05   
    979  2.450000e-05 2017-12-04  2.494000e-05  2.355000e-05  2.417000e-05   
    980  2.568000e-05 2017-12-05  2.636000e-05  2.350000e-05  2.450000e-05   
    981  1.921000e-05 2017-12-06  2.579000e-05  1.786000e-05  2.569000e-05   
    982  1.270000e-05 2017-12-07  2.200000e-05  8.310000e-06  1.923000e-05   
    983  3.800000e-05 2017-12-08  4.430000e-05  1.146000e-05  1.275000e-05   
    984  2.965000e-05 2017-12-09  4.099000e-05  2.420000e-05  3.801000e-05   
    985  2.566000e-05 2017-12-10  3.052000e-05  2.441000e-05  2.954000e-05   
    986  2.930000e-05 2017-12-11  3.058000e-05  2.480000e-05  2.566000e-05   
    987  3.047000e-05 2017-12-12  3.459000e-05  2.769000e-05  2.930000e-05   
    988  3.176000e-05 2017-12-13  3.300000e-05  2.934000e-05  3.047000e-05   
    989  3.690000e-05 2017-12-14  3.809000e-05  3.050000e-05  3.176000e-05   
    990  3.338000e-05 2017-12-15  3.715000e-05  2.864000e-05  3.696000e-05   
    991  3.377000e-05 2017-12-16  4.056000e-05  3.200000e-05  3.336000e-05   
    992  3.600000e-05 2017-12-17  3.845000e-05  3.270000e-05  3.377000e-05   
    993  4.422000e-05 2017-12-18  4.430000e-05  3.558000e-05  3.577000e-05   
    994  5.256000e-05 2017-12-19  5.975000e-05  4.417000e-05  4.422000e-05   
    995  5.533000e-05 2017-12-20  5.999000e-05  4.800000e-05  5.248000e-05   
    996  6.286000e-05 2017-12-21  6.309000e-05  5.535000e-05  5.535000e-05   
    997  5.770000e-05 2017-12-22  6.921000e-05  3.800000e-05  6.287000e-05   
    998  6.260000e-05 2017-12-23  6.428000e-05  5.700000e-05  5.770000e-05   
    999  6.959000e-05 2017-12-24  7.498000e-05  5.850000e-05  6.260000e-05   
    
          quoteVolume       volume  weightedAverage  
    0    5.961381e+07    97.319038     1.630000e-06  
    1    2.384865e+08   264.991041     1.110000e-06  
    2    9.577869e+07   112.841565     1.170000e-06  
    3    6.581851e+07    81.295697     1.230000e-06  
    4    8.535530e+07    92.888274     1.080000e-06  
    5    1.088683e+08   102.780788     9.400000e-07  
    6    8.084315e+07    88.110318     1.080000e-06  
    7    5.678294e+07    57.056392     1.000000e-06  
    8    2.971265e+07    28.261242     9.500000e-07  
    9    4.462053e+07    41.046752     9.100000e-07  
    10   8.488161e+07    71.359707     8.400000e-07  
    11   1.340633e+08    94.152627     7.000000e-07  
    12   3.338856e+07    21.875648     6.500000e-07  
    13   8.796227e+07    51.469061     5.800000e-07  
    14   9.183287e+07    65.956546     7.100000e-07  
    15   4.946101e+07    34.984204     7.000000e-07  
    16   2.933173e+07    20.976925     7.100000e-07  
    17   2.505583e+07    18.658654     7.400000e-07  
    18   5.806771e+07    39.410149     6.700000e-07  
    19   4.713028e+07    31.000185     6.500000e-07  
    20   4.633893e+07    27.607607     5.900000e-07  
    21   2.643870e+07    15.656727     5.900000e-07  
    22   1.672441e+07     9.877157     5.900000e-07  
    23   1.375975e+07     8.008749     5.800000e-07  
    24   2.149122e+07    12.928892     6.000000e-07  
    25   1.707689e+07    10.688681     6.200000e-07  
    26   1.531389e+07     9.231202     6.000000e-07  
    27   4.144198e+07    28.646475     6.900000e-07  
    28   1.770131e+07    11.921024     6.700000e-07  
    29   1.334481e+07     8.670193     6.400000e-07  
    ..            ...          ...              ...  
    970  1.120809e+07   283.822628     2.532000e-05  
    971  1.424404e+07   337.375469     2.368000e-05  
    972  1.036150e+07   233.705501     2.255000e-05  
    973  3.301583e+07   800.688757     2.425000e-05  
    974  3.100064e+07   711.989451     2.296000e-05  
    975  1.405065e+07   315.820787     2.247000e-05  
    976  1.017862e+07   224.934640     2.209000e-05  
    977  2.195081e+07   500.633438     2.280000e-05  
    978  3.598679e+07   873.461128     2.427000e-05  
    979  1.014915e+07   245.801922     2.421000e-05  
    980  1.830850e+07   450.476850     2.460000e-05  
    981  2.456620e+07   546.093959     2.222000e-05  
    982  3.931618e+07   640.238883     1.628000e-05  
    983  1.700135e+08  4683.793100     2.754000e-05  
    984  1.243166e+08  3856.531478     3.102000e-05  
    985  4.149289e+07  1106.329934     2.666000e-05  
    986  3.350860e+07   915.111805     2.730000e-05  
    987  4.792287e+07  1485.412243     3.099000e-05  
    988  2.683856e+07   846.139372     3.152000e-05  
    989  4.566795e+07  1533.002861     3.356000e-05  
    990  3.265831e+07  1054.403373     3.228000e-05  
    991  3.812237e+07  1346.339082     3.531000e-05  
    992  2.787419e+07   989.518893     3.549000e-05  
    993  3.561917e+07  1440.167619     4.043000e-05  
    994  8.180908e+07  4195.546675     5.128000e-05  
    995  3.626382e+07  1939.848138     5.349000e-05  
    996  3.282530e+07  1947.078129     5.931000e-05  
    997  9.727428e+07  5463.712891     5.616000e-05  
    998  2.011290e+07  1225.158713     6.091000e-05  
    999  1.834749e+07  1195.511251     6.515000e-05  
    
    [1000 rows x 8 columns]])




```python
altcoin_data.keys()
```




    dict_keys(['ETH', 'LTC', 'XRP', 'ETC', 'STR', 'DASH', 'SC', 'XMR', 'XEM'])




```python
# Average BTC price calculation
btc_usd_datasets['avg_btc_price_usd'] = btc_usd_datasets.mean(axis=1)
```


```python
# NOTE: for calculation of 2 dataframes, always ensure that the index col is same. In this case, index col is 'date'. Otherwise, one will find NaN values.
for altcoin in altcoin_data.keys():
    altcoin_data[altcoin]['price_usd'] = altcoin_data[altcoin]['weightedAverage'] * btc_usd_datasets['avg_btc_price_usd']
```


```python
altcoin_data['ETH'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>close</th>
      <th>high</th>
      <th>low</th>
      <th>open</th>
      <th>quoteVolume</th>
      <th>volume</th>
      <th>weightedAverage</th>
      <th>price_usd</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-08-08</th>
      <td>0.003125</td>
      <td>50.000000</td>
      <td>0.002620</td>
      <td>50.000000</td>
      <td>2.662061e+05</td>
      <td>1205.803321</td>
      <td>0.004530</td>
      <td>1.224162</td>
    </tr>
    <tr>
      <th>2015-08-09</th>
      <td>0.002581</td>
      <td>0.004100</td>
      <td>0.002400</td>
      <td>0.003000</td>
      <td>3.139879e+05</td>
      <td>898.123434</td>
      <td>0.002860</td>
      <td>0.755301</td>
    </tr>
    <tr>
      <th>2015-08-10</th>
      <td>0.002645</td>
      <td>0.002902</td>
      <td>0.002200</td>
      <td>0.002650</td>
      <td>2.845754e+05</td>
      <td>718.365266</td>
      <td>0.002524</td>
      <td>0.669491</td>
    </tr>
    <tr>
      <th>2015-08-11</th>
      <td>0.003950</td>
      <td>0.004400</td>
      <td>0.002414</td>
      <td>0.002650</td>
      <td>9.151385e+05</td>
      <td>3007.274111</td>
      <td>0.003286</td>
      <td>0.874050</td>
    </tr>
    <tr>
      <th>2015-08-12</th>
      <td>0.004500</td>
      <td>0.004882</td>
      <td>0.002910</td>
      <td>0.003955</td>
      <td>1.117821e+06</td>
      <td>4690.075032</td>
      <td>0.004196</td>
      <td>1.127745</td>
    </tr>
  </tbody>
</table>
</div>




```python
btc_usd_datasets.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BITSTAMP</th>
      <th>COINBASE</th>
      <th>ITBIT</th>
      <th>KRAKEN</th>
      <th>avg_btc_price_usd</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2011-09-13</th>
      <td>5.929231</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.929231</td>
    </tr>
    <tr>
      <th>2011-09-14</th>
      <td>5.590798</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.590798</td>
    </tr>
    <tr>
      <th>2011-09-15</th>
      <td>5.094272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.094272</td>
    </tr>
    <tr>
      <th>2011-09-16</th>
      <td>4.854515</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.854515</td>
    </tr>
    <tr>
      <th>2011-09-17</th>
      <td>4.870000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.870000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Visualize the data from 'June 2015' to 'Dec 2017'
btc_usd_datasets_2015to2017 = btc_usd_datasets[btc_usd_datasets.index.isin(pd.date_range('2015-06-01', '2017-12-21'))]
btc_usd_datasets_2015to2017
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BITSTAMP</th>
      <th>COINBASE</th>
      <th>ITBIT</th>
      <th>KRAKEN</th>
      <th>avg_btc_price_usd</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-06-01</th>
      <td>225.321483</td>
      <td>226.180112</td>
      <td>224.528628</td>
      <td>228.353960</td>
      <td>226.096046</td>
    </tr>
    <tr>
      <th>2015-06-02</th>
      <td>223.760288</td>
      <td>224.776699</td>
      <td>222.635773</td>
      <td>228.035013</td>
      <td>224.801943</td>
    </tr>
    <tr>
      <th>2015-06-03</th>
      <td>225.074874</td>
      <td>226.046511</td>
      <td>224.831949</td>
      <td>226.269642</td>
      <td>225.555744</td>
    </tr>
    <tr>
      <th>2015-06-04</th>
      <td>224.524634</td>
      <td>225.164634</td>
      <td>224.538680</td>
      <td>225.632203</td>
      <td>224.965038</td>
    </tr>
    <tr>
      <th>2015-06-05</th>
      <td>223.244899</td>
      <td>225.004642</td>
      <td>224.285560</td>
      <td>222.835197</td>
      <td>223.842575</td>
    </tr>
    <tr>
      <th>2015-06-06</th>
      <td>224.422835</td>
      <td>224.990557</td>
      <td>224.016036</td>
      <td>225.148317</td>
      <td>224.644436</td>
    </tr>
    <tr>
      <th>2015-06-07</th>
      <td>223.938073</td>
      <td>224.476357</td>
      <td>223.748881</td>
      <td>224.673373</td>
      <td>224.209171</td>
    </tr>
    <tr>
      <th>2015-06-08</th>
      <td>225.979290</td>
      <td>227.260662</td>
      <td>225.271296</td>
      <td>226.818111</td>
      <td>226.332340</td>
    </tr>
    <tr>
      <th>2015-06-09</th>
      <td>229.188716</td>
      <td>230.097674</td>
      <td>229.245768</td>
      <td>227.037682</td>
      <td>228.892460</td>
    </tr>
    <tr>
      <th>2015-06-10</th>
      <td>228.309009</td>
      <td>229.059050</td>
      <td>228.584960</td>
      <td>227.788793</td>
      <td>228.435453</td>
    </tr>
    <tr>
      <th>2015-06-11</th>
      <td>228.876792</td>
      <td>229.690520</td>
      <td>229.193810</td>
      <td>230.356869</td>
      <td>229.529498</td>
    </tr>
    <tr>
      <th>2015-06-12</th>
      <td>229.737159</td>
      <td>230.063732</td>
      <td>229.097165</td>
      <td>230.127007</td>
      <td>229.756266</td>
    </tr>
    <tr>
      <th>2015-06-13</th>
      <td>230.579464</td>
      <td>231.309953</td>
      <td>230.654938</td>
      <td>228.973444</td>
      <td>230.379450</td>
    </tr>
    <tr>
      <th>2015-06-14</th>
      <td>232.260908</td>
      <td>233.681045</td>
      <td>232.350983</td>
      <td>231.717821</td>
      <td>232.502689</td>
    </tr>
    <tr>
      <th>2015-06-15</th>
      <td>234.462336</td>
      <td>236.454881</td>
      <td>235.099521</td>
      <td>233.055425</td>
      <td>234.768041</td>
    </tr>
    <tr>
      <th>2015-06-16</th>
      <td>242.923908</td>
      <td>244.496039</td>
      <td>245.333626</td>
      <td>244.985946</td>
      <td>244.434880</td>
    </tr>
    <tr>
      <th>2015-06-17</th>
      <td>250.320260</td>
      <td>251.953095</td>
      <td>249.918342</td>
      <td>250.781559</td>
      <td>250.743314</td>
    </tr>
    <tr>
      <th>2015-06-18</th>
      <td>246.461818</td>
      <td>249.525007</td>
      <td>248.887161</td>
      <td>245.762564</td>
      <td>247.659138</td>
    </tr>
    <tr>
      <th>2015-06-19</th>
      <td>246.266591</td>
      <td>248.338389</td>
      <td>248.737072</td>
      <td>246.503579</td>
      <td>247.461407</td>
    </tr>
    <tr>
      <th>2015-06-20</th>
      <td>242.490749</td>
      <td>244.212413</td>
      <td>243.757413</td>
      <td>243.222091</td>
      <td>243.420666</td>
    </tr>
    <tr>
      <th>2015-06-21</th>
      <td>243.160155</td>
      <td>244.356169</td>
      <td>242.357384</td>
      <td>241.014283</td>
      <td>242.721997</td>
    </tr>
    <tr>
      <th>2015-06-22</th>
      <td>245.432913</td>
      <td>246.437294</td>
      <td>245.789967</td>
      <td>245.681433</td>
      <td>245.835402</td>
    </tr>
    <tr>
      <th>2015-06-23</th>
      <td>244.357766</td>
      <td>245.214140</td>
      <td>244.017213</td>
      <td>244.136141</td>
      <td>244.431315</td>
    </tr>
    <tr>
      <th>2015-06-24</th>
      <td>241.240275</td>
      <td>242.115980</td>
      <td>242.755981</td>
      <td>242.372476</td>
      <td>242.121178</td>
    </tr>
    <tr>
      <th>2015-06-25</th>
      <td>241.378478</td>
      <td>241.927682</td>
      <td>241.037564</td>
      <td>241.614048</td>
      <td>241.489443</td>
    </tr>
    <tr>
      <th>2015-06-26</th>
      <td>241.741129</td>
      <td>241.899461</td>
      <td>241.132463</td>
      <td>242.106936</td>
      <td>241.719997</td>
    </tr>
    <tr>
      <th>2015-06-27</th>
      <td>247.411024</td>
      <td>247.143120</td>
      <td>247.370822</td>
      <td>246.879584</td>
      <td>247.201137</td>
    </tr>
    <tr>
      <th>2015-06-28</th>
      <td>249.314484</td>
      <td>249.871524</td>
      <td>249.236313</td>
      <td>250.610854</td>
      <td>249.758294</td>
    </tr>
    <tr>
      <th>2015-06-29</th>
      <td>253.791680</td>
      <td>254.311739</td>
      <td>253.002880</td>
      <td>257.878199</td>
      <td>254.746125</td>
    </tr>
    <tr>
      <th>2015-06-30</th>
      <td>262.387741</td>
      <td>262.953257</td>
      <td>261.288164</td>
      <td>265.008184</td>
      <td>262.909337</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-11-22</th>
      <td>8196.453479</td>
      <td>8196.486620</td>
      <td>8167.900743</td>
      <td>8203.215967</td>
      <td>8191.014202</td>
    </tr>
    <tr>
      <th>2017-11-23</th>
      <td>8147.263648</td>
      <td>8181.136948</td>
      <td>8163.003921</td>
      <td>8156.897201</td>
      <td>8162.075430</td>
    </tr>
    <tr>
      <th>2017-11-24</th>
      <td>8154.139542</td>
      <td>8182.040668</td>
      <td>8172.930940</td>
      <td>8059.146344</td>
      <td>8142.064374</td>
    </tr>
    <tr>
      <th>2017-11-25</th>
      <td>8407.830677</td>
      <td>8522.558911</td>
      <td>8580.367255</td>
      <td>8447.542912</td>
      <td>8489.574939</td>
    </tr>
    <tr>
      <th>2017-11-26</th>
      <td>9027.857089</td>
      <td>9206.919540</td>
      <td>9184.919496</td>
      <td>9023.042380</td>
      <td>9110.684626</td>
    </tr>
    <tr>
      <th>2017-11-27</th>
      <td>9527.863617</td>
      <td>9651.360612</td>
      <td>9592.928385</td>
      <td>9494.206005</td>
      <td>9566.589655</td>
    </tr>
    <tr>
      <th>2017-11-28</th>
      <td>9852.647901</td>
      <td>9892.840318</td>
      <td>9836.883769</td>
      <td>9727.223970</td>
      <td>9827.398990</td>
    </tr>
    <tr>
      <th>2017-11-29</th>
      <td>10377.663143</td>
      <td>10368.578491</td>
      <td>10405.182638</td>
      <td>10275.273668</td>
      <td>10356.674485</td>
    </tr>
    <tr>
      <th>2017-11-30</th>
      <td>9715.306791</td>
      <td>9940.329483</td>
      <td>9704.881677</td>
      <td>9894.191618</td>
      <td>9813.677392</td>
    </tr>
    <tr>
      <th>2017-12-01</th>
      <td>10287.373047</td>
      <td>10293.184141</td>
      <td>10364.119476</td>
      <td>10296.380966</td>
      <td>10310.264407</td>
    </tr>
    <tr>
      <th>2017-12-02</th>
      <td>10927.843358</td>
      <td>10946.056300</td>
      <td>10929.867866</td>
      <td>10926.747343</td>
      <td>10932.628717</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>11249.499550</td>
      <td>11317.806361</td>
      <td>11241.321426</td>
      <td>11274.703356</td>
      <td>11270.832673</td>
    </tr>
    <tr>
      <th>2017-12-04</th>
      <td>11314.695288</td>
      <td>11389.214464</td>
      <td>11322.744359</td>
      <td>11387.470641</td>
      <td>11353.531188</td>
    </tr>
    <tr>
      <th>2017-12-05</th>
      <td>11652.183797</td>
      <td>11730.844593</td>
      <td>11665.127516</td>
      <td>11691.906471</td>
      <td>11685.015594</td>
    </tr>
    <tr>
      <th>2017-12-06</th>
      <td>12664.260424</td>
      <td>12934.987064</td>
      <td>12621.943104</td>
      <td>12698.571846</td>
      <td>12729.940609</td>
    </tr>
    <tr>
      <th>2017-12-07</th>
      <td>14840.190945</td>
      <td>15913.273462</td>
      <td>14934.320571</td>
      <td>14895.165334</td>
      <td>15145.737578</td>
    </tr>
    <tr>
      <th>2017-12-08</th>
      <td>15285.203654</td>
      <td>16148.287136</td>
      <td>15501.486877</td>
      <td>15482.664986</td>
      <td>15604.410664</td>
    </tr>
    <tr>
      <th>2017-12-09</th>
      <td>14402.981914</td>
      <td>15263.356753</td>
      <td>14838.105341</td>
      <td>14689.946026</td>
      <td>14798.597508</td>
    </tr>
    <tr>
      <th>2017-12-10</th>
      <td>14233.606090</td>
      <td>14840.534690</td>
      <td>14645.296051</td>
      <td>14479.575113</td>
      <td>14549.752986</td>
    </tr>
    <tr>
      <th>2017-12-11</th>
      <td>16293.316148</td>
      <td>16677.130456</td>
      <td>16737.434053</td>
      <td>16492.422087</td>
      <td>16550.075686</td>
    </tr>
    <tr>
      <th>2017-12-12</th>
      <td>16854.872561</td>
      <td>17240.079853</td>
      <td>17069.065069</td>
      <td>16822.261755</td>
      <td>16996.569810</td>
    </tr>
    <tr>
      <th>2017-12-13</th>
      <td>16434.926066</td>
      <td>17003.219046</td>
      <td>16797.592463</td>
      <td>16548.671920</td>
      <td>16696.102374</td>
    </tr>
    <tr>
      <th>2017-12-14</th>
      <td>16351.302832</td>
      <td>16754.664503</td>
      <td>16563.514194</td>
      <td>16285.497696</td>
      <td>16488.744806</td>
    </tr>
    <tr>
      <th>2017-12-15</th>
      <td>17334.997706</td>
      <td>17500.585655</td>
      <td>17439.672284</td>
      <td>17348.352057</td>
      <td>17405.901926</td>
    </tr>
    <tr>
      <th>2017-12-16</th>
      <td>18430.689989</td>
      <td>18767.431192</td>
      <td>18644.058698</td>
      <td>18405.941445</td>
      <td>18562.030331</td>
    </tr>
    <tr>
      <th>2017-12-17</th>
      <td>19110.244062</td>
      <td>19455.628104</td>
      <td>19247.277924</td>
      <td>19135.469160</td>
      <td>19237.154813</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>18634.418590</td>
      <td>18779.790859</td>
      <td>18640.346919</td>
      <td>18683.675327</td>
      <td>18684.557924</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>17986.053806</td>
      <td>18046.801749</td>
      <td>17934.823780</td>
      <td>18093.126238</td>
      <td>18015.201393</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>16724.840365</td>
      <td>16627.512506</td>
      <td>16644.736331</td>
      <td>16515.544624</td>
      <td>16628.158457</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>16033.611116</td>
      <td>16072.079918</td>
      <td>15940.292149</td>
      <td>15707.989038</td>
      <td>15938.493055</td>
    </tr>
  </tbody>
</table>
<p>935 rows × 5 columns</p>
</div>




```python
# Visualize the data from 'June 2015' to 'Dec 2017'
altcoin_data_2015to2017 = {}

for altcoin in altcoin_data.keys():
    altcoin_data_2015to2017[altcoin] = altcoin_data[altcoin][altcoin_data[altcoin].index.isin(pd.date_range('2015-06-01', '2017-12-21'))]
```


```python
altcoin_data_2015to2017['ETH']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>close</th>
      <th>high</th>
      <th>low</th>
      <th>open</th>
      <th>quoteVolume</th>
      <th>volume</th>
      <th>weightedAverage</th>
      <th>price_usd</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-08-08</th>
      <td>0.003125</td>
      <td>50.000000</td>
      <td>0.002620</td>
      <td>50.000000</td>
      <td>2.662061e+05</td>
      <td>1205.803321</td>
      <td>0.004530</td>
      <td>1.224162</td>
    </tr>
    <tr>
      <th>2015-08-09</th>
      <td>0.002581</td>
      <td>0.004100</td>
      <td>0.002400</td>
      <td>0.003000</td>
      <td>3.139879e+05</td>
      <td>898.123434</td>
      <td>0.002860</td>
      <td>0.755301</td>
    </tr>
    <tr>
      <th>2015-08-10</th>
      <td>0.002645</td>
      <td>0.002902</td>
      <td>0.002200</td>
      <td>0.002650</td>
      <td>2.845754e+05</td>
      <td>718.365266</td>
      <td>0.002524</td>
      <td>0.669491</td>
    </tr>
    <tr>
      <th>2015-08-11</th>
      <td>0.003950</td>
      <td>0.004400</td>
      <td>0.002414</td>
      <td>0.002650</td>
      <td>9.151385e+05</td>
      <td>3007.274111</td>
      <td>0.003286</td>
      <td>0.874050</td>
    </tr>
    <tr>
      <th>2015-08-12</th>
      <td>0.004500</td>
      <td>0.004882</td>
      <td>0.002910</td>
      <td>0.003955</td>
      <td>1.117821e+06</td>
      <td>4690.075032</td>
      <td>0.004196</td>
      <td>1.127745</td>
    </tr>
    <tr>
      <th>2015-08-13</th>
      <td>0.006955</td>
      <td>0.007999</td>
      <td>0.004010</td>
      <td>0.004500</td>
      <td>1.361099e+06</td>
      <td>8192.675011</td>
      <td>0.006019</td>
      <td>1.596377</td>
    </tr>
    <tr>
      <th>2015-08-14</th>
      <td>0.006831</td>
      <td>0.008888</td>
      <td>0.006500</td>
      <td>0.006880</td>
      <td>1.531205e+06</td>
      <td>11651.548987</td>
      <td>0.007609</td>
      <td>2.021394</td>
    </tr>
    <tr>
      <th>2015-08-15</th>
      <td>0.006377</td>
      <td>0.007205</td>
      <td>0.005700</td>
      <td>0.006858</td>
      <td>7.361567e+05</td>
      <td>4745.484895</td>
      <td>0.006446</td>
      <td>1.712331</td>
    </tr>
    <tr>
      <th>2015-08-16</th>
      <td>0.006190</td>
      <td>0.006559</td>
      <td>0.004150</td>
      <td>0.006377</td>
      <td>1.469308e+06</td>
      <td>7666.273099</td>
      <td>0.005218</td>
      <td>1.354970</td>
    </tr>
    <tr>
      <th>2015-08-17</th>
      <td>0.004747</td>
      <td>0.006195</td>
      <td>0.004395</td>
      <td>0.006190</td>
      <td>8.906068e+05</td>
      <td>4618.675968</td>
      <td>0.005186</td>
      <td>1.341226</td>
    </tr>
    <tr>
      <th>2015-08-18</th>
      <td>0.005139</td>
      <td>0.005372</td>
      <td>0.004251</td>
      <td>0.004685</td>
      <td>8.989845e+05</td>
      <td>4321.566218</td>
      <td>0.004807</td>
      <td>1.178080</td>
    </tr>
    <tr>
      <th>2015-08-19</th>
      <td>0.005500</td>
      <td>0.005700</td>
      <td>0.005005</td>
      <td>0.005140</td>
      <td>6.890972e+05</td>
      <td>3728.056758</td>
      <td>0.005410</td>
      <td>1.254231</td>
    </tr>
    <tr>
      <th>2015-08-20</th>
      <td>0.006220</td>
      <td>0.006570</td>
      <td>0.005228</td>
      <td>0.005500</td>
      <td>1.229657e+06</td>
      <td>7236.153172</td>
      <td>0.005885</td>
      <td>1.375320</td>
    </tr>
    <tr>
      <th>2015-08-21</th>
      <td>0.005959</td>
      <td>0.006777</td>
      <td>0.005711</td>
      <td>0.006250</td>
      <td>9.009119e+05</td>
      <td>5677.528871</td>
      <td>0.006302</td>
      <td>1.475977</td>
    </tr>
    <tr>
      <th>2015-08-22</th>
      <td>0.005953</td>
      <td>0.006490</td>
      <td>0.005805</td>
      <td>0.005959</td>
      <td>3.704242e+05</td>
      <td>2285.268266</td>
      <td>0.006169</td>
      <td>1.407928</td>
    </tr>
    <tr>
      <th>2015-08-23</th>
      <td>0.005889</td>
      <td>0.006358</td>
      <td>0.005599</td>
      <td>0.005953</td>
      <td>7.356438e+05</td>
      <td>4325.617489</td>
      <td>0.005880</td>
      <td>1.350845</td>
    </tr>
    <tr>
      <th>2015-08-24</th>
      <td>0.005834</td>
      <td>0.006145</td>
      <td>0.005770</td>
      <td>0.005959</td>
      <td>3.193668e+05</td>
      <td>1887.470634</td>
      <td>0.005910</td>
      <td>1.289791</td>
    </tr>
    <tr>
      <th>2015-08-25</th>
      <td>0.005134</td>
      <td>0.005942</td>
      <td>0.005000</td>
      <td>0.005834</td>
      <td>6.256742e+05</td>
      <td>3373.828192</td>
      <td>0.005392</td>
      <td>1.158191</td>
    </tr>
    <tr>
      <th>2015-08-26</th>
      <td>0.005190</td>
      <td>0.005284</td>
      <td>0.004773</td>
      <td>0.005134</td>
      <td>4.877377e+05</td>
      <td>2463.805429</td>
      <td>0.005051</td>
      <td>1.151170</td>
    </tr>
    <tr>
      <th>2015-08-27</th>
      <td>0.005100</td>
      <td>0.005275</td>
      <td>0.004984</td>
      <td>0.005135</td>
      <td>2.720249e+05</td>
      <td>1399.600139</td>
      <td>0.005145</td>
      <td>1.167442</td>
    </tr>
    <tr>
      <th>2015-08-28</th>
      <td>0.005100</td>
      <td>0.005230</td>
      <td>0.005010</td>
      <td>0.005100</td>
      <td>1.990655e+05</td>
      <td>1020.524557</td>
      <td>0.005127</td>
      <td>1.177989</td>
    </tr>
    <tr>
      <th>2015-08-29</th>
      <td>0.005145</td>
      <td>0.005200</td>
      <td>0.005000</td>
      <td>0.005100</td>
      <td>1.802156e+05</td>
      <td>918.455330</td>
      <td>0.005096</td>
      <td>1.176311</td>
    </tr>
    <tr>
      <th>2015-08-30</th>
      <td>0.005750</td>
      <td>0.006051</td>
      <td>0.005065</td>
      <td>0.005145</td>
      <td>6.004443e+05</td>
      <td>3426.154790</td>
      <td>0.005706</td>
      <td>1.306319</td>
    </tr>
    <tr>
      <th>2015-08-31</th>
      <td>0.005912</td>
      <td>0.006350</td>
      <td>0.005226</td>
      <td>0.005750</td>
      <td>6.401329e+05</td>
      <td>3731.085653</td>
      <td>0.005829</td>
      <td>1.336580</td>
    </tr>
    <tr>
      <th>2015-09-01</th>
      <td>0.005924</td>
      <td>0.006140</td>
      <td>0.005817</td>
      <td>0.005900</td>
      <td>3.185666e+05</td>
      <td>1907.926470</td>
      <td>0.005989</td>
      <td>1.368114</td>
    </tr>
    <tr>
      <th>2015-09-02</th>
      <td>0.005660</td>
      <td>0.005968</td>
      <td>0.005459</td>
      <td>0.005924</td>
      <td>3.743889e+05</td>
      <td>2147.969002</td>
      <td>0.005737</td>
      <td>1.312541</td>
    </tr>
    <tr>
      <th>2015-09-03</th>
      <td>0.005500</td>
      <td>0.005739</td>
      <td>0.005204</td>
      <td>0.005658</td>
      <td>3.319758e+05</td>
      <td>1806.284582</td>
      <td>0.005441</td>
      <td>1.240465</td>
    </tr>
    <tr>
      <th>2015-09-04</th>
      <td>0.005509</td>
      <td>0.005732</td>
      <td>0.005355</td>
      <td>0.005500</td>
      <td>2.139500e+05</td>
      <td>1190.493443</td>
      <td>0.005564</td>
      <td>1.278176</td>
    </tr>
    <tr>
      <th>2015-09-05</th>
      <td>0.005699</td>
      <td>0.005730</td>
      <td>0.005500</td>
      <td>0.005510</td>
      <td>1.214705e+05</td>
      <td>683.539477</td>
      <td>0.005627</td>
      <td>1.316373</td>
    </tr>
    <tr>
      <th>2015-09-06</th>
      <td>0.005383</td>
      <td>0.005740</td>
      <td>0.005369</td>
      <td>0.005650</td>
      <td>1.524807e+05</td>
      <td>838.524738</td>
      <td>0.005499</td>
      <td>1.322887</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-11-22</th>
      <td>0.046345</td>
      <td>0.046355</td>
      <td>0.044110</td>
      <td>0.044501</td>
      <td>1.004890e+05</td>
      <td>4509.438917</td>
      <td>0.044875</td>
      <td>367.571107</td>
    </tr>
    <tr>
      <th>2017-11-23</th>
      <td>0.050921</td>
      <td>0.052201</td>
      <td>0.045711</td>
      <td>0.046340</td>
      <td>2.409020e+05</td>
      <td>11830.206856</td>
      <td>0.049108</td>
      <td>400.822874</td>
    </tr>
    <tr>
      <th>2017-11-24</th>
      <td>0.057462</td>
      <td>0.058461</td>
      <td>0.048080</td>
      <td>0.050780</td>
      <td>2.997200e+05</td>
      <td>16084.125410</td>
      <td>0.053664</td>
      <td>436.934277</td>
    </tr>
    <tr>
      <th>2017-11-25</th>
      <td>0.052845</td>
      <td>0.059121</td>
      <td>0.052845</td>
      <td>0.057462</td>
      <td>1.878513e+05</td>
      <td>10450.760172</td>
      <td>0.055633</td>
      <td>472.301796</td>
    </tr>
    <tr>
      <th>2017-11-26</th>
      <td>0.050586</td>
      <td>0.052994</td>
      <td>0.047662</td>
      <td>0.052845</td>
      <td>2.027793e+05</td>
      <td>10231.093743</td>
      <td>0.050454</td>
      <td>459.673489</td>
    </tr>
    <tr>
      <th>2017-11-27</th>
      <td>0.048807</td>
      <td>0.050900</td>
      <td>0.048500</td>
      <td>0.050586</td>
      <td>1.175349e+05</td>
      <td>5819.785201</td>
      <td>0.049515</td>
      <td>473.693035</td>
    </tr>
    <tr>
      <th>2017-11-28</th>
      <td>0.046956</td>
      <td>0.049416</td>
      <td>0.046336</td>
      <td>0.048716</td>
      <td>1.296472e+05</td>
      <td>6185.746635</td>
      <td>0.047712</td>
      <td>468.886236</td>
    </tr>
    <tr>
      <th>2017-11-29</th>
      <td>0.043200</td>
      <td>0.047906</td>
      <td>0.043047</td>
      <td>0.046956</td>
      <td>2.604961e+05</td>
      <td>11763.896535</td>
      <td>0.045160</td>
      <td>467.703173</td>
    </tr>
    <tr>
      <th>2017-11-30</th>
      <td>0.043770</td>
      <td>0.044845</td>
      <td>0.042450</td>
      <td>0.043300</td>
      <td>1.331669e+05</td>
      <td>5801.811208</td>
      <td>0.043568</td>
      <td>427.562100</td>
    </tr>
    <tr>
      <th>2017-12-01</th>
      <td>0.042447</td>
      <td>0.044571</td>
      <td>0.042202</td>
      <td>0.043615</td>
      <td>8.286531e+04</td>
      <td>3589.518042</td>
      <td>0.043317</td>
      <td>446.614775</td>
    </tr>
    <tr>
      <th>2017-12-02</th>
      <td>0.041811</td>
      <td>0.042657</td>
      <td>0.041600</td>
      <td>0.042447</td>
      <td>5.906807e+04</td>
      <td>2484.743072</td>
      <td>0.042066</td>
      <td>459.889226</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>0.041286</td>
      <td>0.042200</td>
      <td>0.040400</td>
      <td>0.041966</td>
      <td>9.070096e+04</td>
      <td>3744.967920</td>
      <td>0.041289</td>
      <td>465.363326</td>
    </tr>
    <tr>
      <th>2017-12-04</th>
      <td>0.040090</td>
      <td>0.041247</td>
      <td>0.039802</td>
      <td>0.041120</td>
      <td>6.667896e+04</td>
      <td>2702.009705</td>
      <td>0.040523</td>
      <td>460.075398</td>
    </tr>
    <tr>
      <th>2017-12-05</th>
      <td>0.038974</td>
      <td>0.040430</td>
      <td>0.038500</td>
      <td>0.040090</td>
      <td>8.192597e+04</td>
      <td>3220.563718</td>
      <td>0.039311</td>
      <td>459.345558</td>
    </tr>
    <tr>
      <th>2017-12-06</th>
      <td>0.030330</td>
      <td>0.038966</td>
      <td>0.029003</td>
      <td>0.038801</td>
      <td>3.375624e+05</td>
      <td>11393.113521</td>
      <td>0.033751</td>
      <td>429.649880</td>
    </tr>
    <tr>
      <th>2017-12-07</th>
      <td>0.024883</td>
      <td>0.032321</td>
      <td>0.024714</td>
      <td>0.030330</td>
      <td>4.458287e+05</td>
      <td>12328.163369</td>
      <td>0.027652</td>
      <td>418.813570</td>
    </tr>
    <tr>
      <th>2017-12-08</th>
      <td>0.027759</td>
      <td>0.031700</td>
      <td>0.023700</td>
      <td>0.024883</td>
      <td>4.798754e+05</td>
      <td>13334.945566</td>
      <td>0.027788</td>
      <td>433.620825</td>
    </tr>
    <tr>
      <th>2017-12-09</th>
      <td>0.031287</td>
      <td>0.034813</td>
      <td>0.027500</td>
      <td>0.027880</td>
      <td>2.832214e+05</td>
      <td>8972.838448</td>
      <td>0.031681</td>
      <td>468.839547</td>
    </tr>
    <tr>
      <th>2017-12-10</th>
      <td>0.028639</td>
      <td>0.032980</td>
      <td>0.027783</td>
      <td>0.031304</td>
      <td>2.060106e+05</td>
      <td>6245.219863</td>
      <td>0.030315</td>
      <td>441.076198</td>
    </tr>
    <tr>
      <th>2017-12-11</th>
      <td>0.030700</td>
      <td>0.031085</td>
      <td>0.026823</td>
      <td>0.028636</td>
      <td>1.911329e+05</td>
      <td>5433.180530</td>
      <td>0.028426</td>
      <td>470.455430</td>
    </tr>
    <tr>
      <th>2017-12-12</th>
      <td>0.037440</td>
      <td>0.039023</td>
      <td>0.029710</td>
      <td>0.030710</td>
      <td>4.582798e+05</td>
      <td>15686.724043</td>
      <td>0.034230</td>
      <td>581.785276</td>
    </tr>
    <tr>
      <th>2017-12-13</th>
      <td>0.042715</td>
      <td>0.043738</td>
      <td>0.036000</td>
      <td>0.037706</td>
      <td>3.943022e+05</td>
      <td>15872.300955</td>
      <td>0.040254</td>
      <td>672.087409</td>
    </tr>
    <tr>
      <th>2017-12-14</th>
      <td>0.041824</td>
      <td>0.046063</td>
      <td>0.040000</td>
      <td>0.042880</td>
      <td>3.015209e+05</td>
      <td>12936.404634</td>
      <td>0.042904</td>
      <td>707.430304</td>
    </tr>
    <tr>
      <th>2017-12-15</th>
      <td>0.038701</td>
      <td>0.042100</td>
      <td>0.034300</td>
      <td>0.041824</td>
      <td>2.424363e+05</td>
      <td>9156.088428</td>
      <td>0.037767</td>
      <td>657.368350</td>
    </tr>
    <tr>
      <th>2017-12-16</th>
      <td>0.035485</td>
      <td>0.040799</td>
      <td>0.034772</td>
      <td>0.038701</td>
      <td>1.638981e+05</td>
      <td>6153.492990</td>
      <td>0.037545</td>
      <td>696.904375</td>
    </tr>
    <tr>
      <th>2017-12-17</th>
      <td>0.037600</td>
      <td>0.038620</td>
      <td>0.035490</td>
      <td>0.035501</td>
      <td>1.391539e+05</td>
      <td>5153.693210</td>
      <td>0.037036</td>
      <td>712.465534</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>0.041560</td>
      <td>0.042915</td>
      <td>0.037124</td>
      <td>0.037600</td>
      <td>1.735592e+05</td>
      <td>6906.643679</td>
      <td>0.039794</td>
      <td>743.536101</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>0.046195</td>
      <td>0.047125</td>
      <td>0.040800</td>
      <td>0.041400</td>
      <td>2.521555e+05</td>
      <td>11360.369368</td>
      <td>0.045053</td>
      <td>811.639229</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>0.048405</td>
      <td>0.049291</td>
      <td>0.044710</td>
      <td>0.046195</td>
      <td>2.324343e+05</td>
      <td>10913.157781</td>
      <td>0.046952</td>
      <td>780.718146</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>0.050461</td>
      <td>0.051600</td>
      <td>0.047519</td>
      <td>0.048400</td>
      <td>1.884621e+05</td>
      <td>9360.662383</td>
      <td>0.049669</td>
      <td>791.643752</td>
    </tr>
  </tbody>
</table>
<p>867 rows × 8 columns</p>
</div>




```python
# Now, to combine the dataframes of 'altcoin_data' for each crypto, but only for 'price_usd' column
combined_df_2015to2017 = merge_dfs_on_column(list(altcoin_data_2015to2017.values()), list(altcoin_data_2015to2017.keys()), 'price_usd')
```


```python
combined_df_2015to2017
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DASH</th>
      <th>ETC</th>
      <th>ETH</th>
      <th>LTC</th>
      <th>SC</th>
      <th>STR</th>
      <th>XEM</th>
      <th>XMR</th>
      <th>XRP</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-06-01</th>
      <td>2.832063</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.624878</td>
      <td>NaN</td>
      <td>0.003150</td>
      <td>0.000170</td>
      <td>0.456988</td>
      <td>0.007540</td>
    </tr>
    <tr>
      <th>2015-06-02</th>
      <td>2.757502</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.637284</td>
      <td>NaN</td>
      <td>0.003001</td>
      <td>0.000175</td>
      <td>0.452733</td>
      <td>0.007702</td>
    </tr>
    <tr>
      <th>2015-06-03</th>
      <td>2.825065</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.679648</td>
      <td>NaN</td>
      <td>0.003068</td>
      <td>0.000174</td>
      <td>0.456917</td>
      <td>0.008086</td>
    </tr>
    <tr>
      <th>2015-06-04</th>
      <td>2.786784</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.664210</td>
      <td>NaN</td>
      <td>0.003120</td>
      <td>0.000171</td>
      <td>0.470411</td>
      <td>0.007667</td>
    </tr>
    <tr>
      <th>2015-06-05</th>
      <td>2.756791</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.697799</td>
      <td>NaN</td>
      <td>0.003055</td>
      <td>0.000170</td>
      <td>0.487090</td>
      <td>0.007823</td>
    </tr>
    <tr>
      <th>2015-06-06</th>
      <td>2.826121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.742843</td>
      <td>NaN</td>
      <td>0.003084</td>
      <td>0.000164</td>
      <td>0.513739</td>
      <td>0.007813</td>
    </tr>
    <tr>
      <th>2015-06-07</th>
      <td>2.821672</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.732614</td>
      <td>NaN</td>
      <td>0.003058</td>
      <td>0.000161</td>
      <td>0.513778</td>
      <td>0.007894</td>
    </tr>
    <tr>
      <th>2015-06-08</th>
      <td>2.838896</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.763441</td>
      <td>NaN</td>
      <td>0.003078</td>
      <td>0.000165</td>
      <td>0.527730</td>
      <td>0.007883</td>
    </tr>
    <tr>
      <th>2015-06-09</th>
      <td>2.790270</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.805582</td>
      <td>NaN</td>
      <td>0.003200</td>
      <td>0.000151</td>
      <td>0.493037</td>
      <td>0.008016</td>
    </tr>
    <tr>
      <th>2015-06-10</th>
      <td>2.703739</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.771709</td>
      <td>NaN</td>
      <td>0.003175</td>
      <td>0.000144</td>
      <td>0.484502</td>
      <td>0.007977</td>
    </tr>
    <tr>
      <th>2015-06-11</th>
      <td>2.722766</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.774509</td>
      <td>NaN</td>
      <td>0.003062</td>
      <td>0.000142</td>
      <td>0.469016</td>
      <td>0.008031</td>
    </tr>
    <tr>
      <th>2015-06-12</th>
      <td>2.825140</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.771476</td>
      <td>NaN</td>
      <td>0.003030</td>
      <td>0.000142</td>
      <td>0.472308</td>
      <td>0.007876</td>
    </tr>
    <tr>
      <th>2015-06-13</th>
      <td>2.785619</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.833696</td>
      <td>NaN</td>
      <td>0.003108</td>
      <td>0.000147</td>
      <td>0.474577</td>
      <td>0.007927</td>
    </tr>
    <tr>
      <th>2015-06-14</th>
      <td>2.770649</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.987542</td>
      <td>NaN</td>
      <td>0.003209</td>
      <td>0.000149</td>
      <td>0.487377</td>
      <td>0.008093</td>
    </tr>
    <tr>
      <th>2015-06-15</th>
      <td>2.756181</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.990638</td>
      <td>NaN</td>
      <td>0.003244</td>
      <td>0.000143</td>
      <td>0.500293</td>
      <td>0.008400</td>
    </tr>
    <tr>
      <th>2015-06-16</th>
      <td>2.888098</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.510344</td>
      <td>NaN</td>
      <td>0.003515</td>
      <td>0.000154</td>
      <td>0.502617</td>
      <td>0.009311</td>
    </tr>
    <tr>
      <th>2015-06-17</th>
      <td>3.014729</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.999043</td>
      <td>NaN</td>
      <td>0.003671</td>
      <td>0.000163</td>
      <td>0.504927</td>
      <td>0.009551</td>
    </tr>
    <tr>
      <th>2015-06-18</th>
      <td>2.920523</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.027304</td>
      <td>NaN</td>
      <td>0.003539</td>
      <td>0.000161</td>
      <td>0.496032</td>
      <td>0.009909</td>
    </tr>
    <tr>
      <th>2015-06-19</th>
      <td>2.940772</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.850827</td>
      <td>NaN</td>
      <td>0.003759</td>
      <td>0.000156</td>
      <td>0.517264</td>
      <td>0.011047</td>
    </tr>
    <tr>
      <th>2015-06-20</th>
      <td>2.906516</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.919037</td>
      <td>NaN</td>
      <td>0.003576</td>
      <td>0.000151</td>
      <td>0.533999</td>
      <td>0.010844</td>
    </tr>
    <tr>
      <th>2015-06-21</th>
      <td>2.857986</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.980228</td>
      <td>NaN</td>
      <td>0.003585</td>
      <td>0.000155</td>
      <td>0.526884</td>
      <td>0.010340</td>
    </tr>
    <tr>
      <th>2015-06-22</th>
      <td>2.886791</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.007014</td>
      <td>NaN</td>
      <td>0.003582</td>
      <td>0.000160</td>
      <td>0.529124</td>
      <td>0.010234</td>
    </tr>
    <tr>
      <th>2015-06-23</th>
      <td>2.861953</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.979950</td>
      <td>NaN</td>
      <td>0.003552</td>
      <td>0.000154</td>
      <td>0.525317</td>
      <td>0.010506</td>
    </tr>
    <tr>
      <th>2015-06-24</th>
      <td>2.837718</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.872543</td>
      <td>NaN</td>
      <td>0.003489</td>
      <td>0.000153</td>
      <td>0.513205</td>
      <td>0.011569</td>
    </tr>
    <tr>
      <th>2015-06-25</th>
      <td>2.821932</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.818109</td>
      <td>NaN</td>
      <td>0.003366</td>
      <td>0.000152</td>
      <td>0.502952</td>
      <td>0.011034</td>
    </tr>
    <tr>
      <th>2015-06-26</th>
      <td>2.835395</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.848984</td>
      <td>NaN</td>
      <td>0.003425</td>
      <td>0.000150</td>
      <td>0.484172</td>
      <td>0.011267</td>
    </tr>
    <tr>
      <th>2015-06-27</th>
      <td>2.783181</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.986583</td>
      <td>NaN</td>
      <td>0.003374</td>
      <td>0.000151</td>
      <td>0.489285</td>
      <td>0.011732</td>
    </tr>
    <tr>
      <th>2015-06-28</th>
      <td>2.590193</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.081952</td>
      <td>NaN</td>
      <td>0.003424</td>
      <td>0.000150</td>
      <td>0.467273</td>
      <td>0.011252</td>
    </tr>
    <tr>
      <th>2015-06-29</th>
      <td>2.755663</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.578027</td>
      <td>NaN</td>
      <td>0.003388</td>
      <td>0.000153</td>
      <td>0.486262</td>
      <td>0.011815</td>
    </tr>
    <tr>
      <th>2015-06-30</th>
      <td>2.833140</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.162780</td>
      <td>NaN</td>
      <td>0.003481</td>
      <td>0.000152</td>
      <td>0.492174</td>
      <td>0.011292</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-11-22</th>
      <td>553.924625</td>
      <td>17.821517</td>
      <td>367.571107</td>
      <td>70.805912</td>
      <td>0.004915</td>
      <td>0.039808</td>
      <td>0.202728</td>
      <td>157.036732</td>
      <td>0.235901</td>
    </tr>
    <tr>
      <th>2017-11-23</th>
      <td>563.904895</td>
      <td>18.307290</td>
      <td>400.822874</td>
      <td>73.821973</td>
      <td>0.005224</td>
      <td>0.041871</td>
      <td>0.202175</td>
      <td>162.786146</td>
      <td>0.243720</td>
    </tr>
    <tr>
      <th>2017-11-24</th>
      <td>556.366962</td>
      <td>19.088174</td>
      <td>436.934277</td>
      <td>74.392577</td>
      <td>0.004804</td>
      <td>0.039896</td>
      <td>0.202982</td>
      <td>157.879513</td>
      <td>0.239947</td>
    </tr>
    <tr>
      <th>2017-11-25</th>
      <td>611.908696</td>
      <td>21.570991</td>
      <td>472.301796</td>
      <td>82.183840</td>
      <td>0.005179</td>
      <td>0.042108</td>
      <td>0.214956</td>
      <td>165.170878</td>
      <td>0.249509</td>
    </tr>
    <tr>
      <th>2017-11-26</th>
      <td>625.408686</td>
      <td>21.533285</td>
      <td>459.673489</td>
      <td>86.315355</td>
      <td>0.005284</td>
      <td>0.046920</td>
      <td>0.215741</td>
      <td>163.138925</td>
      <td>0.250179</td>
    </tr>
    <tr>
      <th>2017-11-27</th>
      <td>622.382520</td>
      <td>23.543760</td>
      <td>473.693035</td>
      <td>89.007646</td>
      <td>0.005931</td>
      <td>0.053860</td>
      <td>0.215727</td>
      <td>164.036112</td>
      <td>0.247392</td>
    </tr>
    <tr>
      <th>2017-11-28</th>
      <td>615.604193</td>
      <td>30.058377</td>
      <td>468.886236</td>
      <td>92.020030</td>
      <td>0.006191</td>
      <td>0.067023</td>
      <td>0.238314</td>
      <td>183.515375</td>
      <td>0.262883</td>
    </tr>
    <tr>
      <th>2017-11-29</th>
      <td>681.570987</td>
      <td>28.113607</td>
      <td>467.703173</td>
      <td>95.076240</td>
      <td>0.006110</td>
      <td>0.081714</td>
      <td>0.237789</td>
      <td>187.250021</td>
      <td>0.260056</td>
    </tr>
    <tr>
      <th>2017-11-30</th>
      <td>733.360410</td>
      <td>24.981992</td>
      <td>427.562100</td>
      <td>84.092911</td>
      <td>0.005398</td>
      <td>0.070168</td>
      <td>0.220513</td>
      <td>172.652910</td>
      <td>0.233467</td>
    </tr>
    <tr>
      <th>2017-12-01</th>
      <td>772.036001</td>
      <td>27.732962</td>
      <td>446.614775</td>
      <td>90.608253</td>
      <td>0.005671</td>
      <td>0.080214</td>
      <td>0.227754</td>
      <td>181.183101</td>
      <td>0.241982</td>
    </tr>
    <tr>
      <th>2017-12-02</th>
      <td>767.372142</td>
      <td>29.537448</td>
      <td>459.889226</td>
      <td>99.668622</td>
      <td>0.006232</td>
      <td>0.094567</td>
      <td>0.249264</td>
      <td>191.198229</td>
      <td>0.245984</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>769.450392</td>
      <td>29.323889</td>
      <td>465.363326</td>
      <td>100.534588</td>
      <td>0.008115</td>
      <td>0.091406</td>
      <td>0.273543</td>
      <td>201.323896</td>
      <td>0.246719</td>
    </tr>
    <tr>
      <th>2017-12-04</th>
      <td>755.868605</td>
      <td>28.852502</td>
      <td>460.075398</td>
      <td>99.548102</td>
      <td>0.010218</td>
      <td>0.092077</td>
      <td>0.274869</td>
      <td>199.358244</td>
      <td>0.244669</td>
    </tr>
    <tr>
      <th>2017-12-05</th>
      <td>753.562916</td>
      <td>28.601880</td>
      <td>459.345558</td>
      <td>101.233600</td>
      <td>0.009582</td>
      <td>0.111942</td>
      <td>0.287451</td>
      <td>229.036588</td>
      <td>0.239660</td>
    </tr>
    <tr>
      <th>2017-12-06</th>
      <td>709.221526</td>
      <td>26.585463</td>
      <td>429.649880</td>
      <td>96.749967</td>
      <td>0.009038</td>
      <td>0.138629</td>
      <td>0.282859</td>
      <td>268.538988</td>
      <td>0.225829</td>
    </tr>
    <tr>
      <th>2017-12-07</th>
      <td>673.336933</td>
      <td>25.204931</td>
      <td>418.813570</td>
      <td>93.801945</td>
      <td>0.008330</td>
      <td>0.141764</td>
      <td>0.246573</td>
      <td>257.408020</td>
      <td>0.214615</td>
    </tr>
    <tr>
      <th>2017-12-08</th>
      <td>698.726030</td>
      <td>24.789479</td>
      <td>433.620825</td>
      <td>114.276873</td>
      <td>0.007802</td>
      <td>0.126084</td>
      <td>0.429745</td>
      <td>254.102379</td>
      <td>0.227668</td>
    </tr>
    <tr>
      <th>2017-12-09</th>
      <td>726.188786</td>
      <td>27.979708</td>
      <td>468.839547</td>
      <td>141.385801</td>
      <td>0.008139</td>
      <td>0.135259</td>
      <td>0.459052</td>
      <td>262.429153</td>
      <td>0.239441</td>
    </tr>
    <tr>
      <th>2017-12-10</th>
      <td>682.773057</td>
      <td>25.974656</td>
      <td>441.076198</td>
      <td>141.498967</td>
      <td>0.007566</td>
      <td>0.117853</td>
      <td>0.387896</td>
      <td>242.156340</td>
      <td>0.234397</td>
    </tr>
    <tr>
      <th>2017-12-11</th>
      <td>727.192617</td>
      <td>27.479746</td>
      <td>470.455430</td>
      <td>187.325838</td>
      <td>0.008441</td>
      <td>0.137531</td>
      <td>0.451817</td>
      <td>262.925591</td>
      <td>0.242790</td>
    </tr>
    <tr>
      <th>2017-12-12</th>
      <td>848.233192</td>
      <td>29.391998</td>
      <td>581.785276</td>
      <td>275.232424</td>
      <td>0.009348</td>
      <td>0.147190</td>
      <td>0.526724</td>
      <td>295.814420</td>
      <td>0.333133</td>
    </tr>
    <tr>
      <th>2017-12-13</th>
      <td>883.030141</td>
      <td>30.244822</td>
      <td>672.087409</td>
      <td>306.106174</td>
      <td>0.010185</td>
      <td>0.151100</td>
      <td>0.526261</td>
      <td>310.788763</td>
      <td>0.436603</td>
    </tr>
    <tr>
      <th>2017-12-14</th>
      <td>923.982926</td>
      <td>31.394900</td>
      <td>707.430304</td>
      <td>286.516344</td>
      <td>0.010553</td>
      <td>0.175110</td>
      <td>0.553362</td>
      <td>319.087057</td>
      <td>0.658890</td>
    </tr>
    <tr>
      <th>2017-12-15</th>
      <td>873.650432</td>
      <td>29.693598</td>
      <td>657.368350</td>
      <td>278.565795</td>
      <td>0.010444</td>
      <td>0.181021</td>
      <td>0.561863</td>
      <td>304.948269</td>
      <td>0.736618</td>
    </tr>
    <tr>
      <th>2017-12-16</th>
      <td>922.453091</td>
      <td>36.216563</td>
      <td>696.904375</td>
      <td>298.799313</td>
      <td>0.011508</td>
      <td>0.206224</td>
      <td>0.655425</td>
      <td>325.657457</td>
      <td>0.765127</td>
    </tr>
    <tr>
      <th>2017-12-17</th>
      <td>1079.964830</td>
      <td>34.229247</td>
      <td>712.465534</td>
      <td>316.327117</td>
      <td>0.015390</td>
      <td>0.264126</td>
      <td>0.682727</td>
      <td>341.419869</td>
      <td>0.725433</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>1093.617265</td>
      <td>37.237390</td>
      <td>743.536101</td>
      <td>329.416791</td>
      <td>0.014948</td>
      <td>0.265881</td>
      <td>0.755417</td>
      <td>350.778285</td>
      <td>0.731127</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>1155.879642</td>
      <td>39.012279</td>
      <td>811.639229</td>
      <td>346.905402</td>
      <td>0.015313</td>
      <td>0.272750</td>
      <td>0.923820</td>
      <td>363.436691</td>
      <td>0.762403</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>1347.509546</td>
      <td>38.341707</td>
      <td>780.718146</td>
      <td>315.366494</td>
      <td>0.018790</td>
      <td>0.235122</td>
      <td>0.889440</td>
      <td>403.312159</td>
      <td>0.701708</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>1402.437089</td>
      <td>38.373994</td>
      <td>791.643752</td>
      <td>302.663058</td>
      <td>0.022633</td>
      <td>0.251669</td>
      <td>0.945312</td>
      <td>425.916859</td>
      <td>0.943081</td>
    </tr>
  </tbody>
</table>
<p>935 rows × 9 columns</p>
</div>




```python
# Add another column 'BTC' to the combined dataframe.
combined_df_2015to2017['BTC'] = btc_usd_datasets_2015to2017['avg_btc_price_usd']
```


```python
combined_df_2015to2017
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DASH</th>
      <th>ETC</th>
      <th>ETH</th>
      <th>LTC</th>
      <th>SC</th>
      <th>STR</th>
      <th>XEM</th>
      <th>XMR</th>
      <th>XRP</th>
      <th>BTC</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-06-01</th>
      <td>2.832063</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.624878</td>
      <td>NaN</td>
      <td>0.003150</td>
      <td>0.000170</td>
      <td>0.456988</td>
      <td>0.007540</td>
      <td>226.096046</td>
    </tr>
    <tr>
      <th>2015-06-02</th>
      <td>2.757502</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.637284</td>
      <td>NaN</td>
      <td>0.003001</td>
      <td>0.000175</td>
      <td>0.452733</td>
      <td>0.007702</td>
      <td>224.801943</td>
    </tr>
    <tr>
      <th>2015-06-03</th>
      <td>2.825065</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.679648</td>
      <td>NaN</td>
      <td>0.003068</td>
      <td>0.000174</td>
      <td>0.456917</td>
      <td>0.008086</td>
      <td>225.555744</td>
    </tr>
    <tr>
      <th>2015-06-04</th>
      <td>2.786784</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.664210</td>
      <td>NaN</td>
      <td>0.003120</td>
      <td>0.000171</td>
      <td>0.470411</td>
      <td>0.007667</td>
      <td>224.965038</td>
    </tr>
    <tr>
      <th>2015-06-05</th>
      <td>2.756791</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.697799</td>
      <td>NaN</td>
      <td>0.003055</td>
      <td>0.000170</td>
      <td>0.487090</td>
      <td>0.007823</td>
      <td>223.842575</td>
    </tr>
    <tr>
      <th>2015-06-06</th>
      <td>2.826121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.742843</td>
      <td>NaN</td>
      <td>0.003084</td>
      <td>0.000164</td>
      <td>0.513739</td>
      <td>0.007813</td>
      <td>224.644436</td>
    </tr>
    <tr>
      <th>2015-06-07</th>
      <td>2.821672</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.732614</td>
      <td>NaN</td>
      <td>0.003058</td>
      <td>0.000161</td>
      <td>0.513778</td>
      <td>0.007894</td>
      <td>224.209171</td>
    </tr>
    <tr>
      <th>2015-06-08</th>
      <td>2.838896</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.763441</td>
      <td>NaN</td>
      <td>0.003078</td>
      <td>0.000165</td>
      <td>0.527730</td>
      <td>0.007883</td>
      <td>226.332340</td>
    </tr>
    <tr>
      <th>2015-06-09</th>
      <td>2.790270</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.805582</td>
      <td>NaN</td>
      <td>0.003200</td>
      <td>0.000151</td>
      <td>0.493037</td>
      <td>0.008016</td>
      <td>228.892460</td>
    </tr>
    <tr>
      <th>2015-06-10</th>
      <td>2.703739</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.771709</td>
      <td>NaN</td>
      <td>0.003175</td>
      <td>0.000144</td>
      <td>0.484502</td>
      <td>0.007977</td>
      <td>228.435453</td>
    </tr>
    <tr>
      <th>2015-06-11</th>
      <td>2.722766</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.774509</td>
      <td>NaN</td>
      <td>0.003062</td>
      <td>0.000142</td>
      <td>0.469016</td>
      <td>0.008031</td>
      <td>229.529498</td>
    </tr>
    <tr>
      <th>2015-06-12</th>
      <td>2.825140</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.771476</td>
      <td>NaN</td>
      <td>0.003030</td>
      <td>0.000142</td>
      <td>0.472308</td>
      <td>0.007876</td>
      <td>229.756266</td>
    </tr>
    <tr>
      <th>2015-06-13</th>
      <td>2.785619</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.833696</td>
      <td>NaN</td>
      <td>0.003108</td>
      <td>0.000147</td>
      <td>0.474577</td>
      <td>0.007927</td>
      <td>230.379450</td>
    </tr>
    <tr>
      <th>2015-06-14</th>
      <td>2.770649</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.987542</td>
      <td>NaN</td>
      <td>0.003209</td>
      <td>0.000149</td>
      <td>0.487377</td>
      <td>0.008093</td>
      <td>232.502689</td>
    </tr>
    <tr>
      <th>2015-06-15</th>
      <td>2.756181</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.990638</td>
      <td>NaN</td>
      <td>0.003244</td>
      <td>0.000143</td>
      <td>0.500293</td>
      <td>0.008400</td>
      <td>234.768041</td>
    </tr>
    <tr>
      <th>2015-06-16</th>
      <td>2.888098</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.510344</td>
      <td>NaN</td>
      <td>0.003515</td>
      <td>0.000154</td>
      <td>0.502617</td>
      <td>0.009311</td>
      <td>244.434880</td>
    </tr>
    <tr>
      <th>2015-06-17</th>
      <td>3.014729</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.999043</td>
      <td>NaN</td>
      <td>0.003671</td>
      <td>0.000163</td>
      <td>0.504927</td>
      <td>0.009551</td>
      <td>250.743314</td>
    </tr>
    <tr>
      <th>2015-06-18</th>
      <td>2.920523</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.027304</td>
      <td>NaN</td>
      <td>0.003539</td>
      <td>0.000161</td>
      <td>0.496032</td>
      <td>0.009909</td>
      <td>247.659138</td>
    </tr>
    <tr>
      <th>2015-06-19</th>
      <td>2.940772</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.850827</td>
      <td>NaN</td>
      <td>0.003759</td>
      <td>0.000156</td>
      <td>0.517264</td>
      <td>0.011047</td>
      <td>247.461407</td>
    </tr>
    <tr>
      <th>2015-06-20</th>
      <td>2.906516</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.919037</td>
      <td>NaN</td>
      <td>0.003576</td>
      <td>0.000151</td>
      <td>0.533999</td>
      <td>0.010844</td>
      <td>243.420666</td>
    </tr>
    <tr>
      <th>2015-06-21</th>
      <td>2.857986</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.980228</td>
      <td>NaN</td>
      <td>0.003585</td>
      <td>0.000155</td>
      <td>0.526884</td>
      <td>0.010340</td>
      <td>242.721997</td>
    </tr>
    <tr>
      <th>2015-06-22</th>
      <td>2.886791</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.007014</td>
      <td>NaN</td>
      <td>0.003582</td>
      <td>0.000160</td>
      <td>0.529124</td>
      <td>0.010234</td>
      <td>245.835402</td>
    </tr>
    <tr>
      <th>2015-06-23</th>
      <td>2.861953</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.979950</td>
      <td>NaN</td>
      <td>0.003552</td>
      <td>0.000154</td>
      <td>0.525317</td>
      <td>0.010506</td>
      <td>244.431315</td>
    </tr>
    <tr>
      <th>2015-06-24</th>
      <td>2.837718</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.872543</td>
      <td>NaN</td>
      <td>0.003489</td>
      <td>0.000153</td>
      <td>0.513205</td>
      <td>0.011569</td>
      <td>242.121178</td>
    </tr>
    <tr>
      <th>2015-06-25</th>
      <td>2.821932</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.818109</td>
      <td>NaN</td>
      <td>0.003366</td>
      <td>0.000152</td>
      <td>0.502952</td>
      <td>0.011034</td>
      <td>241.489443</td>
    </tr>
    <tr>
      <th>2015-06-26</th>
      <td>2.835395</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.848984</td>
      <td>NaN</td>
      <td>0.003425</td>
      <td>0.000150</td>
      <td>0.484172</td>
      <td>0.011267</td>
      <td>241.719997</td>
    </tr>
    <tr>
      <th>2015-06-27</th>
      <td>2.783181</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.986583</td>
      <td>NaN</td>
      <td>0.003374</td>
      <td>0.000151</td>
      <td>0.489285</td>
      <td>0.011732</td>
      <td>247.201137</td>
    </tr>
    <tr>
      <th>2015-06-28</th>
      <td>2.590193</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.081952</td>
      <td>NaN</td>
      <td>0.003424</td>
      <td>0.000150</td>
      <td>0.467273</td>
      <td>0.011252</td>
      <td>249.758294</td>
    </tr>
    <tr>
      <th>2015-06-29</th>
      <td>2.755663</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.578027</td>
      <td>NaN</td>
      <td>0.003388</td>
      <td>0.000153</td>
      <td>0.486262</td>
      <td>0.011815</td>
      <td>254.746125</td>
    </tr>
    <tr>
      <th>2015-06-30</th>
      <td>2.833140</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.162780</td>
      <td>NaN</td>
      <td>0.003481</td>
      <td>0.000152</td>
      <td>0.492174</td>
      <td>0.011292</td>
      <td>262.909337</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-11-22</th>
      <td>553.924625</td>
      <td>17.821517</td>
      <td>367.571107</td>
      <td>70.805912</td>
      <td>0.004915</td>
      <td>0.039808</td>
      <td>0.202728</td>
      <td>157.036732</td>
      <td>0.235901</td>
      <td>8191.014202</td>
    </tr>
    <tr>
      <th>2017-11-23</th>
      <td>563.904895</td>
      <td>18.307290</td>
      <td>400.822874</td>
      <td>73.821973</td>
      <td>0.005224</td>
      <td>0.041871</td>
      <td>0.202175</td>
      <td>162.786146</td>
      <td>0.243720</td>
      <td>8162.075430</td>
    </tr>
    <tr>
      <th>2017-11-24</th>
      <td>556.366962</td>
      <td>19.088174</td>
      <td>436.934277</td>
      <td>74.392577</td>
      <td>0.004804</td>
      <td>0.039896</td>
      <td>0.202982</td>
      <td>157.879513</td>
      <td>0.239947</td>
      <td>8142.064374</td>
    </tr>
    <tr>
      <th>2017-11-25</th>
      <td>611.908696</td>
      <td>21.570991</td>
      <td>472.301796</td>
      <td>82.183840</td>
      <td>0.005179</td>
      <td>0.042108</td>
      <td>0.214956</td>
      <td>165.170878</td>
      <td>0.249509</td>
      <td>8489.574939</td>
    </tr>
    <tr>
      <th>2017-11-26</th>
      <td>625.408686</td>
      <td>21.533285</td>
      <td>459.673489</td>
      <td>86.315355</td>
      <td>0.005284</td>
      <td>0.046920</td>
      <td>0.215741</td>
      <td>163.138925</td>
      <td>0.250179</td>
      <td>9110.684626</td>
    </tr>
    <tr>
      <th>2017-11-27</th>
      <td>622.382520</td>
      <td>23.543760</td>
      <td>473.693035</td>
      <td>89.007646</td>
      <td>0.005931</td>
      <td>0.053860</td>
      <td>0.215727</td>
      <td>164.036112</td>
      <td>0.247392</td>
      <td>9566.589655</td>
    </tr>
    <tr>
      <th>2017-11-28</th>
      <td>615.604193</td>
      <td>30.058377</td>
      <td>468.886236</td>
      <td>92.020030</td>
      <td>0.006191</td>
      <td>0.067023</td>
      <td>0.238314</td>
      <td>183.515375</td>
      <td>0.262883</td>
      <td>9827.398990</td>
    </tr>
    <tr>
      <th>2017-11-29</th>
      <td>681.570987</td>
      <td>28.113607</td>
      <td>467.703173</td>
      <td>95.076240</td>
      <td>0.006110</td>
      <td>0.081714</td>
      <td>0.237789</td>
      <td>187.250021</td>
      <td>0.260056</td>
      <td>10356.674485</td>
    </tr>
    <tr>
      <th>2017-11-30</th>
      <td>733.360410</td>
      <td>24.981992</td>
      <td>427.562100</td>
      <td>84.092911</td>
      <td>0.005398</td>
      <td>0.070168</td>
      <td>0.220513</td>
      <td>172.652910</td>
      <td>0.233467</td>
      <td>9813.677392</td>
    </tr>
    <tr>
      <th>2017-12-01</th>
      <td>772.036001</td>
      <td>27.732962</td>
      <td>446.614775</td>
      <td>90.608253</td>
      <td>0.005671</td>
      <td>0.080214</td>
      <td>0.227754</td>
      <td>181.183101</td>
      <td>0.241982</td>
      <td>10310.264407</td>
    </tr>
    <tr>
      <th>2017-12-02</th>
      <td>767.372142</td>
      <td>29.537448</td>
      <td>459.889226</td>
      <td>99.668622</td>
      <td>0.006232</td>
      <td>0.094567</td>
      <td>0.249264</td>
      <td>191.198229</td>
      <td>0.245984</td>
      <td>10932.628717</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>769.450392</td>
      <td>29.323889</td>
      <td>465.363326</td>
      <td>100.534588</td>
      <td>0.008115</td>
      <td>0.091406</td>
      <td>0.273543</td>
      <td>201.323896</td>
      <td>0.246719</td>
      <td>11270.832673</td>
    </tr>
    <tr>
      <th>2017-12-04</th>
      <td>755.868605</td>
      <td>28.852502</td>
      <td>460.075398</td>
      <td>99.548102</td>
      <td>0.010218</td>
      <td>0.092077</td>
      <td>0.274869</td>
      <td>199.358244</td>
      <td>0.244669</td>
      <td>11353.531188</td>
    </tr>
    <tr>
      <th>2017-12-05</th>
      <td>753.562916</td>
      <td>28.601880</td>
      <td>459.345558</td>
      <td>101.233600</td>
      <td>0.009582</td>
      <td>0.111942</td>
      <td>0.287451</td>
      <td>229.036588</td>
      <td>0.239660</td>
      <td>11685.015594</td>
    </tr>
    <tr>
      <th>2017-12-06</th>
      <td>709.221526</td>
      <td>26.585463</td>
      <td>429.649880</td>
      <td>96.749967</td>
      <td>0.009038</td>
      <td>0.138629</td>
      <td>0.282859</td>
      <td>268.538988</td>
      <td>0.225829</td>
      <td>12729.940609</td>
    </tr>
    <tr>
      <th>2017-12-07</th>
      <td>673.336933</td>
      <td>25.204931</td>
      <td>418.813570</td>
      <td>93.801945</td>
      <td>0.008330</td>
      <td>0.141764</td>
      <td>0.246573</td>
      <td>257.408020</td>
      <td>0.214615</td>
      <td>15145.737578</td>
    </tr>
    <tr>
      <th>2017-12-08</th>
      <td>698.726030</td>
      <td>24.789479</td>
      <td>433.620825</td>
      <td>114.276873</td>
      <td>0.007802</td>
      <td>0.126084</td>
      <td>0.429745</td>
      <td>254.102379</td>
      <td>0.227668</td>
      <td>15604.410664</td>
    </tr>
    <tr>
      <th>2017-12-09</th>
      <td>726.188786</td>
      <td>27.979708</td>
      <td>468.839547</td>
      <td>141.385801</td>
      <td>0.008139</td>
      <td>0.135259</td>
      <td>0.459052</td>
      <td>262.429153</td>
      <td>0.239441</td>
      <td>14798.597508</td>
    </tr>
    <tr>
      <th>2017-12-10</th>
      <td>682.773057</td>
      <td>25.974656</td>
      <td>441.076198</td>
      <td>141.498967</td>
      <td>0.007566</td>
      <td>0.117853</td>
      <td>0.387896</td>
      <td>242.156340</td>
      <td>0.234397</td>
      <td>14549.752986</td>
    </tr>
    <tr>
      <th>2017-12-11</th>
      <td>727.192617</td>
      <td>27.479746</td>
      <td>470.455430</td>
      <td>187.325838</td>
      <td>0.008441</td>
      <td>0.137531</td>
      <td>0.451817</td>
      <td>262.925591</td>
      <td>0.242790</td>
      <td>16550.075686</td>
    </tr>
    <tr>
      <th>2017-12-12</th>
      <td>848.233192</td>
      <td>29.391998</td>
      <td>581.785276</td>
      <td>275.232424</td>
      <td>0.009348</td>
      <td>0.147190</td>
      <td>0.526724</td>
      <td>295.814420</td>
      <td>0.333133</td>
      <td>16996.569810</td>
    </tr>
    <tr>
      <th>2017-12-13</th>
      <td>883.030141</td>
      <td>30.244822</td>
      <td>672.087409</td>
      <td>306.106174</td>
      <td>0.010185</td>
      <td>0.151100</td>
      <td>0.526261</td>
      <td>310.788763</td>
      <td>0.436603</td>
      <td>16696.102374</td>
    </tr>
    <tr>
      <th>2017-12-14</th>
      <td>923.982926</td>
      <td>31.394900</td>
      <td>707.430304</td>
      <td>286.516344</td>
      <td>0.010553</td>
      <td>0.175110</td>
      <td>0.553362</td>
      <td>319.087057</td>
      <td>0.658890</td>
      <td>16488.744806</td>
    </tr>
    <tr>
      <th>2017-12-15</th>
      <td>873.650432</td>
      <td>29.693598</td>
      <td>657.368350</td>
      <td>278.565795</td>
      <td>0.010444</td>
      <td>0.181021</td>
      <td>0.561863</td>
      <td>304.948269</td>
      <td>0.736618</td>
      <td>17405.901926</td>
    </tr>
    <tr>
      <th>2017-12-16</th>
      <td>922.453091</td>
      <td>36.216563</td>
      <td>696.904375</td>
      <td>298.799313</td>
      <td>0.011508</td>
      <td>0.206224</td>
      <td>0.655425</td>
      <td>325.657457</td>
      <td>0.765127</td>
      <td>18562.030331</td>
    </tr>
    <tr>
      <th>2017-12-17</th>
      <td>1079.964830</td>
      <td>34.229247</td>
      <td>712.465534</td>
      <td>316.327117</td>
      <td>0.015390</td>
      <td>0.264126</td>
      <td>0.682727</td>
      <td>341.419869</td>
      <td>0.725433</td>
      <td>19237.154813</td>
    </tr>
    <tr>
      <th>2017-12-18</th>
      <td>1093.617265</td>
      <td>37.237390</td>
      <td>743.536101</td>
      <td>329.416791</td>
      <td>0.014948</td>
      <td>0.265881</td>
      <td>0.755417</td>
      <td>350.778285</td>
      <td>0.731127</td>
      <td>18684.557924</td>
    </tr>
    <tr>
      <th>2017-12-19</th>
      <td>1155.879642</td>
      <td>39.012279</td>
      <td>811.639229</td>
      <td>346.905402</td>
      <td>0.015313</td>
      <td>0.272750</td>
      <td>0.923820</td>
      <td>363.436691</td>
      <td>0.762403</td>
      <td>18015.201393</td>
    </tr>
    <tr>
      <th>2017-12-20</th>
      <td>1347.509546</td>
      <td>38.341707</td>
      <td>780.718146</td>
      <td>315.366494</td>
      <td>0.018790</td>
      <td>0.235122</td>
      <td>0.889440</td>
      <td>403.312159</td>
      <td>0.701708</td>
      <td>16628.158457</td>
    </tr>
    <tr>
      <th>2017-12-21</th>
      <td>1402.437089</td>
      <td>38.373994</td>
      <td>791.643752</td>
      <td>302.663058</td>
      <td>0.022633</td>
      <td>0.251669</td>
      <td>0.945312</td>
      <td>425.916859</td>
      <td>0.943081</td>
      <td>15938.493055</td>
    </tr>
  </tbody>
</table>
<p>935 rows × 10 columns</p>
</div>




```python
combined_df_2015to2017.keys()
```




    Index(['DASH', 'ETC', 'ETH', 'LTC', 'SC', 'STR', 'XEM', 'XMR', 'XRP', 'BTC'], dtype='object')




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




    'file://F:\\Coding\\Python\\Python playground Steemit\\cryptocurrency-analysis\\temp-plot.html'




```python
'''
# Didn't Work. TODO

combined_df_data_trace_2015to2017 = {}

for key in combined_df_2015to2017.keys():
    combined_df_data_trace_2015to2017[key] = go.Scatter(x = combined_df_2015to2017.index, y = combined_df_2015to2017[key], name = key)

'''




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




    'file://F:\\Coding\\Python\\Python playground Steemit\\cryptocurrency-analysis\\Cryptocurrency Prices (USD).html'




```python
# Calculate the pearson correlation coefficients for altcoins in 2016
combined_df_2016 = combined_df_2015to2017[combined_df_2015to2017.index.year == 2016]
combined_df_2016.pct_change().corr(method='pearson')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DASH</th>
      <th>ETC</th>
      <th>ETH</th>
      <th>LTC</th>
      <th>SC</th>
      <th>STR</th>
      <th>XEM</th>
      <th>XMR</th>
      <th>XRP</th>
      <th>BTC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>DASH</th>
      <td>1.000000</td>
      <td>0.003992</td>
      <td>0.122695</td>
      <td>-0.012194</td>
      <td>0.026602</td>
      <td>0.058083</td>
      <td>0.014571</td>
      <td>0.121537</td>
      <td>0.088657</td>
      <td>-0.014040</td>
    </tr>
    <tr>
      <th>ETC</th>
      <td>0.003992</td>
      <td>1.000000</td>
      <td>-0.181991</td>
      <td>-0.131079</td>
      <td>-0.008066</td>
      <td>-0.102654</td>
      <td>-0.080938</td>
      <td>-0.105898</td>
      <td>-0.054095</td>
      <td>-0.170538</td>
    </tr>
    <tr>
      <th>ETH</th>
      <td>0.122695</td>
      <td>-0.181991</td>
      <td>1.000000</td>
      <td>-0.064652</td>
      <td>0.169642</td>
      <td>0.035093</td>
      <td>0.043205</td>
      <td>0.087216</td>
      <td>0.085630</td>
      <td>-0.006502</td>
    </tr>
    <tr>
      <th>LTC</th>
      <td>-0.012194</td>
      <td>-0.131079</td>
      <td>-0.064652</td>
      <td>1.000000</td>
      <td>0.012253</td>
      <td>0.113523</td>
      <td>0.160667</td>
      <td>0.129475</td>
      <td>0.053712</td>
      <td>0.750174</td>
    </tr>
    <tr>
      <th>SC</th>
      <td>0.026602</td>
      <td>-0.008066</td>
      <td>0.169642</td>
      <td>0.012253</td>
      <td>1.000000</td>
      <td>0.143252</td>
      <td>0.106153</td>
      <td>0.047910</td>
      <td>0.021098</td>
      <td>0.035116</td>
    </tr>
    <tr>
      <th>STR</th>
      <td>0.058083</td>
      <td>-0.102654</td>
      <td>0.035093</td>
      <td>0.113523</td>
      <td>0.143252</td>
      <td>1.000000</td>
      <td>0.225132</td>
      <td>0.027998</td>
      <td>0.320116</td>
      <td>0.079075</td>
    </tr>
    <tr>
      <th>XEM</th>
      <td>0.014571</td>
      <td>-0.080938</td>
      <td>0.043205</td>
      <td>0.160667</td>
      <td>0.106153</td>
      <td>0.225132</td>
      <td>1.000000</td>
      <td>0.016438</td>
      <td>0.101326</td>
      <td>0.227674</td>
    </tr>
    <tr>
      <th>XMR</th>
      <td>0.121537</td>
      <td>-0.105898</td>
      <td>0.087216</td>
      <td>0.129475</td>
      <td>0.047910</td>
      <td>0.027998</td>
      <td>0.016438</td>
      <td>1.000000</td>
      <td>0.027649</td>
      <td>0.127520</td>
    </tr>
    <tr>
      <th>XRP</th>
      <td>0.088657</td>
      <td>-0.054095</td>
      <td>0.085630</td>
      <td>0.053712</td>
      <td>0.021098</td>
      <td>0.320116</td>
      <td>0.101326</td>
      <td>0.027649</td>
      <td>1.000000</td>
      <td>0.044161</td>
    </tr>
    <tr>
      <th>BTC</th>
      <td>-0.014040</td>
      <td>-0.170538</td>
      <td>-0.006502</td>
      <td>0.750174</td>
      <td>0.035116</td>
      <td>0.079075</td>
      <td>0.227674</td>
      <td>0.127520</td>
      <td>0.044161</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




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


```python
correlation_heatmap(combined_df_2016.pct_change(), "Cryptocurrency Correlations in 2016")
```


```python
combined_df_2017 = combined_df[combined_df.index.year == 2017]
combined_df_2017.pct_change().corr(method='pearson')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DASH</th>
      <th>ETC</th>
      <th>ETH</th>
      <th>LTC</th>
      <th>SC</th>
      <th>STR</th>
      <th>XEM</th>
      <th>XMR</th>
      <th>XRP</th>
      <th>BTC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>DASH</th>
      <td>1.000000</td>
      <td>0.348496</td>
      <td>0.481321</td>
      <td>0.303805</td>
      <td>0.256081</td>
      <td>0.171226</td>
      <td>0.297711</td>
      <td>0.461343</td>
      <td>0.098612</td>
      <td>0.249491</td>
    </tr>
    <tr>
      <th>ETC</th>
      <td>0.348496</td>
      <td>1.000000</td>
      <td>0.580487</td>
      <td>0.457065</td>
      <td>0.276605</td>
      <td>0.199778</td>
      <td>0.297052</td>
      <td>0.410649</td>
      <td>0.124620</td>
      <td>0.371314</td>
    </tr>
    <tr>
      <th>ETH</th>
      <td>0.481321</td>
      <td>0.580487</td>
      <td>1.000000</td>
      <td>0.413982</td>
      <td>0.352460</td>
      <td>0.249387</td>
      <td>0.376906</td>
      <td>0.530839</td>
      <td>0.222435</td>
      <td>0.373296</td>
    </tr>
    <tr>
      <th>LTC</th>
      <td>0.303805</td>
      <td>0.457065</td>
      <td>0.413982</td>
      <td>1.000000</td>
      <td>0.313201</td>
      <td>0.306252</td>
      <td>0.358276</td>
      <td>0.405577</td>
      <td>0.348254</td>
      <td>0.385041</td>
    </tr>
    <tr>
      <th>SC</th>
      <td>0.256081</td>
      <td>0.276605</td>
      <td>0.352460</td>
      <td>0.313201</td>
      <td>1.000000</td>
      <td>0.404339</td>
      <td>0.293267</td>
      <td>0.344212</td>
      <td>0.251045</td>
      <td>0.294761</td>
    </tr>
    <tr>
      <th>STR</th>
      <td>0.171226</td>
      <td>0.199778</td>
      <td>0.249387</td>
      <td>0.306252</td>
      <td>0.404339</td>
      <td>1.000000</td>
      <td>0.329905</td>
      <td>0.324100</td>
      <td>0.499593</td>
      <td>0.227157</td>
    </tr>
    <tr>
      <th>XEM</th>
      <td>0.297711</td>
      <td>0.297052</td>
      <td>0.376906</td>
      <td>0.358276</td>
      <td>0.293267</td>
      <td>0.329905</td>
      <td>1.000000</td>
      <td>0.308014</td>
      <td>0.275012</td>
      <td>0.304484</td>
    </tr>
    <tr>
      <th>XMR</th>
      <td>0.461343</td>
      <td>0.410649</td>
      <td>0.530839</td>
      <td>0.405577</td>
      <td>0.344212</td>
      <td>0.324100</td>
      <td>0.308014</td>
      <td>1.000000</td>
      <td>0.241682</td>
      <td>0.353221</td>
    </tr>
    <tr>
      <th>XRP</th>
      <td>0.098612</td>
      <td>0.124620</td>
      <td>0.222435</td>
      <td>0.348254</td>
      <td>0.251045</td>
      <td>0.499593</td>
      <td>0.275012</td>
      <td>0.241682</td>
      <td>1.000000</td>
      <td>0.147837</td>
    </tr>
    <tr>
      <th>BTC</th>
      <td>0.249491</td>
      <td>0.371314</td>
      <td>0.373296</td>
      <td>0.385041</td>
      <td>0.294761</td>
      <td>0.227157</td>
      <td>0.304484</td>
      <td>0.353221</td>
      <td>0.147837</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
correlation_heatmap(combined_df_2017.pct_change(), "Cryptocurrency Correlations in 2017")
```
