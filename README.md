# The Zone EA

The Zone EA is a customizable Expert Advisor (EA) designed for Forex trading. It incorporates various technical indicators and allows users to define their own entry signals and hedging strategies. This code serves as a sample implementation of the EA and can be customized to suit individual trading preferences.

## Features

- Customizable entry signals: Users can define their own entry signals based on various indicators such as Moving Averages (MA), Bollinger Bands (BB), Average Directional Movement Index (ADX), and Relative Strength Index (RSI).
- Hedging functionality: Users can implement hedging strategies by placing stop orders above or below the current price.
- Indicator entry signals: Users can also use predefined indicators such as RSI as entry signals.
- Trade management: The EA automatically manages trades and provides features such as setting magic numbers for trade identification.

## Usage

To use the Zone EA, follow these steps:

1. Include the necessary libraries: The EA requires several libraries for indicator calculations and trade management. Make sure to include the following libraries at the beginning of your code:

```mql5
#include <Trade\Trade.mqh>
#include <Timeseries\Series.mqh>
#include <Indicators\MA.mqh>
#include <Indicators\BollingerBands.mqh>
#include <Indicators\ADX.mqh>
#include <Indicators\RSI.mqh>
```

2. Define input parameters: Customize the input parameters according to your trading preferences. The input parameters include Moving Average method, period, Bollinger Bands period and deviation, ADX period, RSI period, and oversold/overbought levels.

```mql5
input ENUM_MA_METHOD MA_Method = MODE_SMA; // Moving Average method
input int MA_Period = 14; // Moving Average period
input int BB_Period = 20; // Bollinger Bands period
input double BB_Deviation = 2.0; // Bollinger Bands deviation
input int ADX_Period = 14; // ADX period
input int RSI_Period = 14; // RSI period
input int RSI_OversoldLevel = 30; // RSI oversold level
input int RSI_OverboughtLevel = 70; // RSI overbought level
```

3. Define global variables: Create global objects for trade management and indicator calculations.

```mql5
CTrade trade; // Trade object
CMA ma; // Moving Average object
CBollingerBands bb; // Bollinger Bands object
CADX adx; // ADX object
CRSI rsi; // RSI object
```

4. Implement custom entry signals: Define your own custom entry signals based on the indicators and input parameters. For example, you can buy when the MA crosses above the BB upper band and the ADX is above a certain threshold.

```mql5
bool CustomEntrySignals()
{
    if (ma.iCustom(_Symbol, _Period, MA_Method, MA_Period, 0) > bb.iBands(_Symbol, _Period, BB_Period, BB_Deviation, PRICE_CLOSE, MODE_UPPER, 0) &&
        adx.iADX(_Symbol, _Period, ADX_Period, PRICE_CLOSE, 0) > 25)
    {
        trade.Buy();
        return true;
    }
    else if (rsi.iRSI(_Symbol, _Period, RSI_Period, PRICE_CLOSE, 0) < RSI_OversoldLevel)
    {
        trade.Sell();
        return true;
    }

    return false;
}
```

5. Implement hedging strategy: Define your hedging strategy using the HedgeEA function. For example, you can place a Buy stop order above the current price.

```mql5
void HedgeEA()
{
    double price = SymbolInfoDouble(_Symbol, SYMBOL_ASK);
    double stopOrderPrice = price + 10 * _Point;
    trade.BuyStop(stopOrderPrice, 0.1, stopOrderPrice - 100 * _Point, stopOrderPrice + 100 * _Point, 'Hedge Buy');
}
```

6. Implement indicator entry signals: Use predefined indicators such as RSI as entry signals.

```mql5
bool IndicatorEntrySignals()
{
    if (rsi.iRSI(_Symbol, _Period, RSI_Period, PRICE_CLOSE, 0) > RSI_OverboughtLevel)
    {
        trade.Sell();
        return true;
    }
    return false;
}
```

7. Main functionality: In the OnTick function, check if custom entry signals, hedging strategy, or indicator entry signals are enabled.

```mql5
void OnTick()
{
    if (CustomEntrySignals())
        return;

    if (HedgeEA())
        return;

    if (IndicatorEntrySignals())
        return;
}
```

8. Initialization and cleanup: In the OnInit function, initialize trade object and indicators. In the OnDeinit function, clean up resources.

```mql5
void OnInit()
{
    trade.SetExpertMagicNumber(123456);

    ma.Create(_Symbol, _Period, MA_Period, MA_Method);
    bb.Create(_Symbol, _Period, BB_Period, BB_Deviation, PRICE_CLOSE);
    adx.Create(_Symbol, _Period, ADX_Period, PRICE_CLOSE);
    rsi.Create(_Symbol, _Period, RSI_Period, PRICE_CLOSE);
}

void OnDeinit(const int reason)
{
    trade.ResetLastError();
    ma.Destroy();
    bb.Destroy();
    adx.Destroy();
    rsi.Destroy();
}
```

9. Start function: In the OnStart function, call the main entry point function.

```mql5
void OnStart()
{
    OnTick();
}
```

Please note that this is a sample code provided by ForexRobotEasy and not the official developer of the product. For detailed reviews, trading results, and to find the official developer of this product, visit [Forex Robot Easy](https://forexroboteasy.com/forex-robot-review/zone-ea-review-optimize-forex-trades-with-customized-entry-signals/).
