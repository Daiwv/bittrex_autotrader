# Bittrex AutoTrader

Bittrex currency exchange autotrading script _in a nutshell_.

## Dependencies

To install the Python script dependencies and generate API documentation:

    $ make

## Configuration options

The following options can be passed as script arguments or defined in a file:

| Option | Description                               | Example                          | Default value |
| -------| ------------------------------------------|----------------------------------|---------------|
| apikey | Bittrex issued API key.                   | XxXxxXXxXxxXxxXxXxxXxXxxXXxXxxXx |               |
| secret | Bittrex issued API secret.                | XxXxxXXxXxxXxxXxXxxXxXxxXXxXxxXx |               |
| market | String literal for the market.            | BTC-XXX                          | BTC-LTC       |
| units  | BUY/SELL total units.                     | 0                                | 1             |
| spread | BUY/SELL markup/markdown percentage.      | 0.0/0.0                          | 0.1/0.1       |
| method | Moving Average calculation method.        | method                           | arithmetic    |
| delay  | Seconds to delay order status requests.   | 0                                | 30            |
| prompt | Require user interaction to begin trading.| False                            | True          |

## Basic usage

To run the script:

    $ ./bittrex_autotrader.py --conf bittrex_autotrader.conf

The default configuration requires the user to decide the first type of trade;

    1. BUY in at markdown (need units to trade)
    2. SELL out at markup (need liquidity)

    Enter your choice as a number or unique substring (Control-C aborts):

Make a choice then press Enter.

The script will then retrieve the latest market rates, calculate an asking price based on your configuration and submit an order to Bittrex.

Once an order has completed, the script will again retrieve the latest market rates and submit a new order of the opposite type.

If an order is cancelled (via the Bittrex Web UI), the script will recalculate based on the latest market rates and submit an order of the same type.

### Running as a service

If you wish to automate the running of the script (using [Supervisor](http://supervisord.org/) for example), set the 'prompt' configuration option to 'False'.

On startup, the script will automatically check for an open order and wait for completion or cancellation before initiating an order of the opposite type.

If you do not have any open orders it will initiate a 'SELL' order by default. If you do not have enough funds to carry out this operation, the script will end.

## Bittrex API

Outside of the basic trading functionality a full implementation of the Bittrex API has been provided for those would want to extend this script.  Runnning `make` will generate the class HTML documentation.

### Usage Example

    #!/usr/bin/env python

    from bittrex_autotrader import BittrexApiRequest

    apiReq = BittrexApiRequest(apikey, secret)
    ticker = apiReq.public_ticker(market)

    print ticker['Ask']

## Developer Notes

* If you are new to cryptocurrencies please, and I stress, **Do NOT use this script**. :skull: You have been warned :skull:
* Certain markets are more volatile than others. It's very easy to get priced out of a market, so choose wisely.
* Based on the defined `spread` you can gain/lose units of value.  I take no responsiblity for your losses.
* New features will be added when I have free time available.  You can motivate me by _donating_ below.

## Donations

If this script makes you ~~money~~ happy then buy me a :beer: using one of the cryptocurrencies below:

    Bitcoin:  1Cvr9aHNmV2riULkfgEqofQtuhxCBe7A16
    Litecoin: LcMKbewQftytYnmsGTk63BW7yPCnUKFNni

## License and Warranty

This package is distributed in the hope that it will be useful, but without any warranty; without even the implied warranty of merchantability or fitness for a particular purpose.

_Bittrex AutoTrader_ is provided under the terms of the [MIT license](http://www.opensource.org/licenses/mit-license.php)

[Bittrex](https://bittrex.com) is a registered trademark of Bittrex, INC

## Author

[Marc S. Brooks](https://github.com/nuxy)
