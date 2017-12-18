## Points covered
* Read csv file
* playing with data i.e. data wrangling/munging - delete index column, delete rows, columns, replacing values using loops, etc.
* Write the dataframe to '.csv' and '.xlsx' formats.
* Merging 2 or more dataframes into 1 excel file into 2 diff. sheets.
```python
import pandas as pd
df_stocks = pd.DataFrame({
    'tickers': ['GOOGL', 'WMT', 'MSFT'],
    'price': [845, 68, 96],
    'pe': [30.37, 14.45, 45.89],
    'eps': [58.56, 85.69, 56.45]
})

df_weather = pd.DataFrame({
    'day': ['1/1/2017', '1/2/2017', '1/3/2017'],
    'temperature': [32, 35, 28],
    'event': ['Rain', 'Sunny', 'Snow'],
})

with pd.ExcelWriter('stocks_weather.xlsx') as writer:
    df_stocks.to_excel(writer, sheet_name = "stocks", index=False)
    df_weather.to_excel(writer, sheet_name = "weather", index= False)

```

## Resources
* Python Pandas Tutorial 4: Read Write Excel CSV File - https://www.youtube.com/watch?v=-0NwrcZOKhQ
* 
