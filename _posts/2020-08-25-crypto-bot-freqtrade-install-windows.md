---
layout: post
title:  "freqtrader part 1: Running your open source crypto bot on Windows 10"
date:   2020-8-25 8:05:55 +0100
image:  /images/10.jpg
tags:   Freqtrader crypto bitcoin trading bot
description: How-to guide to create your own local Windows 10 Freqtrader crypto bot to start to test your crypto strategies today.
---

I've been pretty fascinated about trading and crypto from years ago, money just loses it's value, automated algorithmic trading offers a way your computer work as your personal money manager, just under your orders, crypto promises a world with cheap and secure transaction fees, and a shield for inflation.

As i´m not the only one that thinks about this stuff, there are already some very interesting open source projects that aim to automatize crypto trading.

WARNING: This document has no intention of persuade nobody to invest money in anything. It's just a informative about how running a program already available. I am not responsible in any way for any money you invest, only your hability creating algorithms is.

## About open source crypto trading bots

### What is a crypto bot?
In brief: It's a program made to execute automated orders in a cryptocurrency exchange.

What this means? With this tools we can make a computer locally, or on the cloud buy and sell cryptos based on our strategies.

Most of them also have integrated more tools to develop our strategies like backtracking, plotting the data, or optimize our strategy using some kind of AI.

The money we can earn or loose depends on our strategy, and this is the hard part.

For this guide i'll using freqtrade and will show how to test your own strategies under windows docker.

### Why Freqtrade?

I tried to use Zenbot, but Docker install under Windows was not possible, It's made in Node.js and looks like a nice program.

In the other hand, i already had used freqtade for some months now on a cloud server with dry run and has not given any problem.

It's Python 3 based and well mantained, each time is better doccumented and has more features, and i hadn't found any serious bugs. This plus tha ability to run your tests from Windows make it the best alternative.

## Freqtrader local setup

### Initial requeriments
Updated Windows 10 or Windows 10 Home OS with git already installed
Some knowledge aout the command line
A brain interested in algorithms

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

docker-compose run --rm freqtrade backtesting --strategy-list ADXMomentum AdxSmas ASDTSRockwellTrading  AverageStrategy AwesomeMacd BbandRsi BinHV27 BinHV45 CCIStrategy ClucMay72018 CMCWinner CofiBitStrategy  CombinedBinHAndCluc EMASkipPump Low_BB MACDStrategy MACDStrategy_crossed MultiRSI Quickie Scalp TDSequentialStrategy --timeframe 5m --fee 0.001 --export trades

{% endhighlight %}

This test are just an example, some of the strategies perform better in other timeframes and test here is for nothing, but it will give us a start point and show us which was the best strategy with given condition.

The backtesting tool is also a very powerfull one with a lot of options, giving us the ability to save our results in different file formats. It also doesnt include the reinforced or smooth variant ones. Check the [full docs](https://www.freqtrade.io/en/latest/backtesting/) for more and updated info.

#### General Result
In my case i get this results:

![]({{site.baseurl}}/images/freqtrade_main.JPG)


I can see from all testes strategies my more profitable ones where:
1. Quickie with an impressive ***232%***
2. BbandRsi with a impressive ***200%***
3. BinHV27 with a nice ***130%***
4. MACDStrategy_crossed with also ***126%***


#### Looking closer to each strategy

Testing multiple strategies at multiple coins can be good to have a big idea of them, but we can take a better look one by one. By using the --export trades option, we had created a number of json files under user_data/backtest_results/backtest-result-<strategy>.json and we can inspect them more closely, to see inside of the strategy, what coins where profitable the most for example.

My results look like this for Quickie:

{% highlight shell %}
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

![]({{site.baseurl}}/images/freqtrade_quickie.JPG)

### Recap


If you followed this guide, you should be able to continue testing yout crypto strategies. Good luck

### Aknowledgments

This quick intall guide is based on documentation available on [freqtrade docs](https://www.freqtrade.io/en/latest/) and is way more extensive, and as i said very well documented.

Cheers for all developers maintaining and upgrading this nice program, if this tutorial feel usefull for you feel free to [donate them](https://github.com/freqtrade/freqtrade)

