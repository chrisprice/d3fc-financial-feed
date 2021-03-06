# d3fc-financial-feed

An API for fetching financial time-series data from different sources including Quandl and Coinbase

[Main d3fc package](https://github.com/ScottLogic/d3fc)

## Installation

```bash
npm install d3fc-financial-feed
```

## Example usage - Coinbase

``` javascript

import { feedCoinbase } from 'd3fc-financial-feed';

const coinbase = feedCoinbase()
  .product('BTC-GBP');

coinbase((error, data) => {
  if (error) throw error;
  console.log(data);
});

// [
//   {
//     date: 2016-05-11T08:51:00.000Z,
//     open: 317.99,
//     high: 317.99,
//     low: 317.99,
//     close: 317.99,
//     volume: 0.24
//   },
//   ...
// ]

```

## Example usage - Quandl

``` javascript

import { feedQuandl } from 'd3fc-financial-feed';

const quandl = feedQuandl()
  .database('WIKI')
  .dataset('AAPL')
  .rows(10)
  .descending(true)
  .collapse('weekly');

quandl((error, data) => {
  if (error) throw error;
  console.log(data);
});

// [
//   {
//     date: 2016-05-15T00:00:00.000Z,
//     open: 93.33,
//     high: 93.57,
//     low: 92.11,
//     close: 93.39,
//     volume: 33217012,
//     'ex-Dividend': 0,
//     'split Ratio': 1,
//     'adj. Open': 93.33,
//     'adj. High': 93.57,
//     'adj. Low': 92.11,
//     'adj. Close': 93.39,
//     'adj. Volume': 33217012
//   },
//   ...
// ]

```

# API

## Coinbase

https://docs.exchange.coinbase.com/#get-historic-rates

*fc*.**feedCoinbase**()

Constructs a new coinbase feed.

*feedCoinbase*.**product**([*value*])

If *value* is specified, sets the product id to the specified string and returns this feed instance.
If *value* is not specified, returns the current product id, which defaults to `"BTC-USD"`.

*feedCoinbase*.**start**([*value*])

If *value* is specified, sets the start date to the specified `Date` object and returns this feed instance.
If *value* is not specified, returns the current start date, which defaults to null.

*feedCoinbase*.**end**([*value*])

If *value* is specified, sets the end date to the specified `Date` object and returns this feed instance.
If *value* is not specified, returns the current end date, which defaults to null.

*feedCoinbase*.**granularity**([*value*])

If *value* is specified, sets the granularity to the specified number of seconds and returns this feed instance.
If *value* is not specified, returns the current granularity, which defaults to null.

*feedCoinbase*(*callback*)

Makes a request to the Coinbase API, with *callback* invoked when the request succeeds or fails.
The callback is invoked with two arguments: the error, if any, and the data.
Data returned from the API is mapped to an array of objects with numeric
`open`, `high`, `low`, `close` and `volume` properties, and a `Date` instance `date` property.

## Quandl

https://www.quandl.com/docs/api#datasets

*fc*.**feedQuandl**()

Constructs a new quandl feed.

*feedQuandl*.**database**([*value*])

If *value* is specified, sets the unique database code to the specified string and returns this feed instance.
If *value* is not specified, returns the current database code, which defaults to `"YAHOO"`.

*feedQuandl*.**dataset**([*value*])

If *value* is specified, sets the unique dataset code to the specified string and returns this feed instance.
If *value* is not specified, returns the current dataset code, which defaults to `"GOOG"`.

*feedQuandl*.**apiKey**([*value*])

If *value* is specified, sets the API key to the specified string and returns this feed instance.
This is required for premium set or high frequency requests.
If *value* is not specified, returns the current API key, which defaults to null.

*feedQuandl*.**start**([*value*])

If *value* is specified, sets the start date to the specified `Date` object and returns this feed instance.
If *value* is not specified, returns the current start date, which defaults to null.

*feedQuandl*.**end**([*value*])

If *value* is specified, sets the end date to the specified `Date` object and returns this feed instance.
If *value* is not specified, returns the current end date, which defaults to null.

*feedQuandl*.**rows**([*value*])

If *value* is specified, sets the row limit to the specified number and returns this feed instance.
If *value* is not specified, returns the current row limit, which defaults to null.

*feedQuandl*.**descending**([*value*])

If *value* is specified, sets the ordering of the data (descending if `true`, ascending if `false`) and returns this feed instance.
If *value* is not specified, returns the current ordering value, which defaults to false.

*feedQuandl*.**collapse**([*value*])

If *value* is specified, sets the periodicity of the data ("none", "daily" | "weekly" | "monthly" | "quarterly" | "annual") and returns this feed instance.
If *value* is not specified, returns the current periodicity, which defaults to null.

*feedQuandl*.**columnNameMap**([*value*])

If *value* is specified, sets the function used to map Quandl column names to property names and returns this feed instance.
If *value* is not specified, returns the current column name map, which defaults to *feedQuandl*.defaultColumnNameMap.
If the supplied function returns null for a column, then that column will be omitted from the output data.
Setting *value* to null will preserve the original column names.

*feedQuandl*.**defaultColumnNameMap**

Returns the default column name mapping function. This function returns the supplied column name with a lower case first letter (e.g. it maps "Close" to "close").

*feedQuandl*(*callback*)

Makes a request to the Quandl API, with *callback* invoked when the request succeeds or fails.
The callback is invoked with two arguments: the error, if any, and the data.
Data returned from the API is mapped to an array of objects with properties for all non-null names mapped by *feedQuandl*.columnNameMap, with date column values converted to `Date` instances.
