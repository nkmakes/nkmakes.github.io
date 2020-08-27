---
layout: post
title:  "Freqtrader part 1: Running our open source crypto bot on Windows 10"
date:   2020-8-25 8:05:55 +0100
image:  /images/10.jpg
tags:   Freqtrader crypto bitcoin trading bot
description: How-to guide to create your own local Windows 10 Freqtrader crypto bot to start to test your crypto strategies today.
---


## Intro about open source crypto trading bots

### What is a cripto bot?

### Open source crypto bots as 2020

## Freqtrader local setup

### Initial requeriments
Updated Windows 10 or Windows 10 Home OS with git

### Setting up our Docker enviroment on Win 10

We need first to set up a docker enviroment on Windows 10. If you have Home edition OS, you will need to [install WSL 2 or Windows susbsistem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Before this you need to have your OS updated to the last version to avoid problems.

If you are lucky, you can directly install [Docker install Full Guide](https://docs.docker.com/docker-for-windows/install-windows-home/)


### Installing our bot docker container

#### Basic install

Once we got our Docker Desktop program installed, we need to setup our Docker container, for this we need to move to a folder using the windows terminal.
In my case ill move to C:/Users/yourusername/Onedrive/docker, so i have a backup of everithing on onedrive.

Once there We need to create a folder, in my case called freqtrade, weneed to create the user and file structure



{% highlight shell %}
mkdir ft_userdata
cd ft_userdata/
# Download the docker-compose file from the repository
curl https://raw.githubusercontent.com/freqtrade/freqtrade/develop/docker-compose.yml -o docker-compose.yml

# Pull the freqtrade image
docker-compose pull

# Create user directory structure
docker-compose run --rm freqtrade create-userdir --userdir user_data

# Create configuration - Requires answering interactive questions
docker-compose run --rm freqtrade new-config --config user_data/config.json
{% endhighlight %}





#### Download technical framework
To use all the possibilities qe need to add to the dockerfile the technical framework, for this, we need to add the library to the Dockerfile, in docker-compose.yml edit and add:

{% highlight dockerfile %}
services:
  freqtrade:
    image: freqtrade_custom
    build:
      context: .
      dockerfile: "./Dockerfile.technical"
{% endhighlight %}

Create a newfile called Dockerfile.technical and add the following lines:

{% highlight dockerfile %}
FROM freqtradeorg/freqtrade:develop

RUN apt-get update \
    && apt-get -y install git \
    && apt-get clean \
    && pip install git+https://github.com/freqtrade/technical
{% endhighlight %}

After this, next time we run a docker-compose command it will rebuild the Docker image with added dependencies.


#### Basic configuration

Our config has been created in previous step, but we need to configure it to our needs.
All the base/default config for our bot is in the file /user_data/config.json
Specially we need to change the "pair_whitelist" list of currencies.
This is the pair list we are going to use download the historic data and run our backtests with.

Remember all the pairs should have in the right side your stake_currency.

Also take a look at the others options, as you can change them for each test.

#### Downloading example bot configurations

There are a growing number of strategies developed by the community for freqtrade available at [github](https://github.com/freqtrade/freqtrade-strategies).

You can download them using git, or you can just download as a zip and copy them inside of your /userdata/strategies folder.

### First Backtesting

#### Download data for backtesting

To download our data to resolve our backtesting, we need to use the following command:

{% highlight shell %}
docker-compose run --rm freqtrade download-data -t 5m --days 100 
{% endhighlight %}


This will download the data for the last 100 days, in 5 minute candlesticks, of all the pairs in our whitelist.
This command has many options and is very complete, so please refer to [full documentation](https://www.freqtrade.io/en/latest/data-download/) for updated info.

#### Testing all the available example strategies on our data

To test all the example strategies and take a fast look for ones are the most profitable in our downloaded data (in my case, the last 100 days) from all our whitelist pairs in 5m candlesticks.

This is updated in August 2020, check the strategies list to see if there are any news or changes.

{% highlight shell %}

docker-compose run --rm freqtrade backtesting --strategy-list ADXMomentum AdxSmas ASDTSRockwellTrading  AverageStrategy AwesomeMacd BbandRsi BinHV27 BinHV45 CCIStrategy ClucMay72018 CMCCWinner CofiBitStrategy  CombineBinHAndCluc EMASkipPump Low_BB MACDStrategy MACDStrategy_crossed MultiRSI Quickie ReinforcedAverageStrategy ReinforcedQuickie ReinforcedSmoothScalp Scalp SmoothOperator SmoothScalp TDSequentialStrategy --timeframe 5m --fee 0.001

{% endhighlight %}

This test are just an example, some of the strategies perform better in other timeframes and test here is for nothing, but it will give us a start point and show us which was the best strategy with given condition.

The backtesting tool is also a very powerfull one with a lot of options, giving us the ability to save our results in different file formats. It also doesnt include the reinforced or smooth variant ones. Check the [full docs](https://www.freqtrade.io/en/latest/backtesting/) for more and updated info.

#### General Result
In my case i get this results:

{% highlight shell %}

=============== SUMMARY METRICS ===============
| Metric                | Value               |
|-----------------------+---------------------|
| Backtesting from      | 2020-05-19 02:30:00 |
| Backtesting to        | 2020-08-27 09:55:00 |
| Total trades          | 1769                |
| First trade           | 2020-05-19 02:35:00 |
| First trade Pair      | XTZ/BTC             |
| Total Profit %        | -197.47%            |
| Trades per day        | 17.69               |
| Best day              | 22.59%              |
| Worst day             | -90.06%             |
| Days win/draw/lose    | 50 / 0 / 51         |
| Avg. Duration Winners | 5:13:00             |
| Avg. Duration Loser   | 8:20:00             |
|                       |                     |
| Max Drawdown          | 241.95%             |
| Drawdown Start        | 2020-05-31 11:50:00 |
| Drawdown End          | 2020-08-25 17:05:00 |
| Market change         | 40.0%               |
===============================================

=================================================================== STRATEGY SUMMARY ===================================================================
|             Strategy |   Buys |   Avg Profit % |   Cum Profit % |   Tot Profit BTC |   Tot Profit % |     Avg Duration |   Wins |   Draws |   Losses |
|----------------------+--------+----------------+----------------+------------------+----------------+------------------+--------+---------+----------|
|          ADXMomentum |    646 |          -0.20 |        -128.61 |      -0.01287368 |         -12.86 |          2:31:00 |    293 |       0 |      353 |
|              AdxSmas |   4395 |          -0.15 |        -653.11 |      -0.06537630 |         -65.31 |          2:54:00 |   1424 |       1 |     2970 |
| ASDTSRockwellTrading |  12692 |          -0.22 |       -2787.83 |      -0.27906189 |        -278.78 |          0:43:00 |   2594 |       0 |    10098 |
|      AverageStrategy |   9838 |          -0.23 |       -2238.01 |      -0.22402506 |        -223.80 |          1:26:00 |   1678 |       1 |     8159 |
|          AwesomeMacd |   2971 |          -0.22 |        -648.28 |      -0.06489282 |         -64.83 |          4:57:00 |    647 |       0 |     2324 |
|             BbandRsi |   1023 |           0.20 |         202.58 |       0.02027842 |          20.26 |         13:37:00 |    642 |       0 |      381 |
|              BinHV27 |    588 |           0.22 |         130.77 |       0.01308980 |          13.08 |          4:48:00 |    351 |       0 |      237 |
|              BinHV45 |    135 |           0.29 |          39.81 |       0.00398500 |           3.98 |          3:10:00 |    115 |       0 |       20 |
|          CCIStrategy |    198 |           0.11 |          22.58 |       0.00226002 |           2.26 |         22:29:00 |     66 |       0 |      132 |
|         ClucMay72018 |    107 |           0.45 |          47.97 |       0.00480145 |           4.80 |          0:24:00 |     91 |       0 |       16 |
|            CMCWinner |    842 |          -0.05 |         -38.52 |      -0.00385632 |          -3.85 |          2:54:00 |    400 |     374 |       68 |
|      CofiBitStrategy |   1882 |          -0.14 |        -255.16 |      -0.02554111 |         -25.52 |          0:28:00 |    653 |       1 |     1228 |
|  CombinedBinHAndCluc |    167 |           0.67 |         112.10 |       0.01122137 |          11.21 |          1:09:00 |    122 |       0 |       45 |
|          EMASkipPump |   4997 |          -0.10 |        -488.95 |      -0.04894440 |         -48.90 |          2:56:00 |   2643 |       1 |     2353 |
|               Low_BB |     34 |          -0.64 |         -21.90 |      -0.00219256 |          -2.19 | 3 days, 13:53:00 |      5 |       0 |       29 |
|         MACDStrategy |   1479 |           0.04 |          57.51 |       0.00575680 |           5.75 |         11:31:00 |    989 |       0 |      490 |
| MACDStrategy_crossed |    315 |           0.40 |         126.75 |       0.01268736 |          12.67 |  2 days, 0:10:00 |    289 |       0 |       26 |
|             MultiRSI |   2178 |          -0.18 |        -387.14 |      -0.03875312 |         -38.71 |          2:03:00 |   1155 |       0 |     1023 |
|              Quickie |    504 |           0.46 |         232.03 |       0.02322628 |          23.20 |  1 day, 16:33:00 |    471 |       0 |       33 |
|                Scalp |   1888 |          -0.15 |        -282.38 |      -0.02826617 |         -28.24 |          0:27:00 |    664 |       1 |     1223 |
| TDSequentialStrategy |   1769 |          -0.11 |        -197.47 |      -0.01976711 |         -19.75 |          6:38:00 |    959 |       0 |      810 |
========================================================================================================================================================
{% endhighlight %}

I can see from all testes strategies my more profitable ones where:
1. Quickie with an impressive ***232%***
2. BbandRsi with a impressive ***200%***
3. BinHV27 with a nice ***130%***
4. MACDStrategy_crossed with also ***126%***


#### Looking closer to each strategy

Testing multiple strategies at multiple coins can be good to have a big idea of them, but we can take a better look one by one. By using the --export trades option, we had created a number of json files under user_data/backtest_results/backtest-result-<strategy>.json and we can inspect them more closely, to see inside of the strategy, what coins where profitable the most for example.

My results look like this for Quickie:

{% highlight shell %}
Result for strategy Quickie
============================================================= BACKTESTING REPORT ============================================================
|      Pair |   Buys |   Avg Profit % |   Cum Profit % |   Tot Profit BTC |   Tot Profit % |     Avg Duration |   Wins |   Draws |   Losses |
|-----------+--------+----------------+----------------+------------------+----------------+------------------+--------+---------+----------|
| MATIC/BTC |     70 |           0.25 |          17.54 |       0.00175552 |           1.75 |         19:43:00 |     59 |       0 |       11 |
|   BAT/BTC |     55 |           0.38 |          21.03 |       0.00210529 |           2.10 |   1 day, 0:56:00 |     53 |       0 |        2 |
|   ETH/BTC |     20 |           0.47 |           9.44 |       0.00094464 |           0.94 | 3 days, 16:06:00 |     18 |       0 |        2 |
|   BRD/BTC |     67 |           0.28 |          18.78 |       0.00187973 |           1.88 |         18:13:00 |     63 |       0 |        4 |
|   EOS/BTC |     20 |           0.25 |           5.10 |       0.00051038 |           0.51 |  4 days, 1:50:00 |     19 |       0 |        1 |
|  IOTA/BTC |     37 |           0.67 |          24.87 |       0.00248919 |           2.49 |  1 day, 19:07:00 |     35 |       0 |        2 |
|  LINK/BTC |     39 |          -0.05 |          -2.14 |      -0.00021469 |          -0.21 |  1 day, 12:53:00 |     37 |       0 |        2 |
|   LTC/BTC |     17 |           0.70 |          11.90 |       0.00119158 |           1.19 | 4 days, 19:56:00 |     16 |       0 |        1 |
|   NEO/BTC |     38 |           0.78 |          29.72 |       0.00297509 |           2.97 |  1 day, 10:55:00 |     36 |       0 |        2 |
|   NXS/BTC |     56 |           0.51 |          28.78 |       0.00288134 |           2.88 |         18:44:00 |     53 |       0 |        3 |
|   XMR/BTC |     30 |           0.67 |          20.15 |       0.00201701 |           2.01 |  2 days, 5:46:00 |     28 |       0 |        2 |
|   XRP/BTC |     18 |           0.28 |           5.04 |       0.00050476 |           0.50 | 4 days, 16:19:00 |     17 |       0 |        1 |
|   XTZ/BTC |     37 |           1.13 |          41.82 |       0.00418644 |           4.18 |  1 day, 22:52:00 |     37 |       0 |        0 |
|     TOTAL |    504 |           0.46 |         232.03 |       0.02322628 |          23.20 |  1 day, 16:33:00 |    471 |       0 |       33 |
====================================================== SELL REASON STATS ======================================================
|   Sell Reason |   Sells |   Wins |   Draws |   Losses |   Avg Profit % |   Cum Profit % |   Tot Profit BTC |   Tot Profit % |
|---------------+---------+--------+---------+----------+----------------+----------------+------------------+----------------|
|           roi |     471 |    471 |       0 |        0 |           1.13 |         531.44 |       0.0531971  |          53.14 |
|   sell_signal |      16 |      0 |       0 |       16 |          -2.34 |         -37.51 |      -0.00375511 |          -3.75 |
|    force_sell |      10 |      0 |       0 |       10 |          -8.58 |         -85.85 |      -0.00859316 |          -8.58 |
|     stop_loss |       7 |      0 |       0 |        7 |         -25.15 |        -176.05 |      -0.0176225  |         -17.6  |
========================================================== LEFT OPEN TRADES REPORT ==========================================================
|      Pair |   Buys |   Avg Profit % |   Cum Profit % |   Tot Profit BTC |   Tot Profit % |     Avg Duration |   Wins |   Draws |   Losses |
|-----------+--------+----------------+----------------+------------------+----------------+------------------+--------+---------+----------|
| MATIC/BTC |      1 |          -7.61 |          -7.61 |      -0.00076145 |          -0.76 | 9 days, 22:20:00 |      0 |       0 |        1 |
|   BAT/BTC |      1 |         -14.87 |         -14.87 |      -0.00148853 |          -1.49 |  3 days, 1:10:00 |      0 |       0 |        1 |
|   ETH/BTC |      1 |          -7.96 |          -7.96 |      -0.00079651 |          -0.80 | 12 days, 2:05:00 |      0 |       0 |        1 |
|   BRD/BTC |      1 |          -2.12 |          -2.12 |      -0.00021196 |          -0.21 | 2 days, 21:10:00 |      0 |       0 |        1 |
|   EOS/BTC |      1 |         -13.88 |         -13.88 |      -0.00138962 |          -1.39 | 9 days, 20:00:00 |      0 |       0 |        1 |
|  IOTA/BTC |      1 |         -11.30 |         -11.30 |      -0.00113160 |          -1.13 | 5 days, 21:15:00 |      0 |       0 |        1 |
|   LTC/BTC |      1 |          -4.08 |          -4.08 |      -0.00040842 |          -0.41 | 7 days, 23:05:00 |      0 |       0 |        1 |
|   NEO/BTC |      1 |          -3.10 |          -3.10 |      -0.00030993 |          -0.31 | 2 days, 22:50:00 |      0 |       0 |        1 |
|   XMR/BTC |      1 |          -8.02 |          -8.02 |      -0.00080283 |          -0.80 | 5 days, 20:55:00 |      0 |       0 |        1 |
|   XRP/BTC |      1 |         -12.91 |         -12.91 |      -0.00129231 |          -1.29 | 23 days, 5:55:00 |      0 |       0 |        1 |
|     TOTAL |     10 |          -8.58 |         -85.85 |      -0.00859316 |          -8.58 |  8 days, 8:52:00 |      0 |       0 |       10 |
=============== SUMMARY METRICS ===============
| Metric                | Value               |
|-----------------------+---------------------|
| Backtesting from      | 2020-05-19 02:30:00 |
| Backtesting to        | 2020-08-27 09:55:00 |
| Total trades          | 504                 |
| First trade           | 2020-05-19 23:25:00 |
| First trade Pair      | IOTA/BTC            |
| Total Profit %        | 232.03%             |
| Trades per day        | 5.04                |
| Best day              | 16.5%               |
| Worst day             | -112.42%            |
| Days win/draw/lose    | 74 / 15 / 11        |
| Avg. Duration Winners | 1 day, 9:54:00      |
| Avg. Duration Loser   | 5 days, 15:34:00    |
|                       |                     |
| Max Drawdown          | 119.97%             |
| Drawdown Start        | 2020-07-26 20:40:00 |
| Drawdown End          | 2020-07-27 21:50:00 |
| Market change         | 40.0%               |
===============================================
{% endhighlight %}

### Recap


![Hass homeassistant timelapse photo check]({{site.baseurl}}/images/hass_service_test.png)


