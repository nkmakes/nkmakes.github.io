---
layout: post
title:  "Crypto bots part 0: What is a crypto bot and comparison of open source projects"
date:   2020-10-18 8:05:55 +0100
image:  /images/freqtrade_main.JPG
tags:   Freqtrader crypto bitcoin trading bot
description: Introduction to state-of-art of automated crypto trading tools, also known as crypto bots as of Fall 2020. Overview of major crypto projects and comparison table.
---

Introduction to state-of-art of automated crypto trading tools, also known as crypto bots as of Fall 2020. Overview of major crypto projects and comparison table.

*** WARNING: This document has no intention of persuade nobody to invest money in anything. It's just a informative about how running a program already available. I am not responsible in any way for any money you invest, only your ability creating algorithms is. ***

## About open source crypto trading bots

### What is a crypto bot?
In brief: It's a library or program made to execute automated orders in a cryptocurrency exchange.

What this means? With this tools we can make a computer locally, or on the cloud buy and sell cryptos based on our strategies.

Most of them also have integrated more tools to develop our strategies like backtracking, plotting the data, or optimize our strategy using some kind of AI.

The money we can earn or loose depends on our strategy, and this is the hard part. 


### Overview and selected projects

For anybody interested, i am going to compare major open source crypto bots projects based on github. I selected 6 based on the following two points:

Their code is fully open-source, excluiding free Web platforms as Quantopian, Quantiacs, QuantConnect or WorldQuant.

Their code has been updated in the last year on their main repo, excluiding all the ones that are not actually mantained like magic8bot, gekko, bitprophet and PHPTradingbot.

This lets us with the following projects:

- [Freqtrade](https://github.com/freqtrade/freqtrade)
- [Zenbot](https://github.com/DeviaVir/zenbot)
- [Jesse-AI](https://github.com/jesse-ai/jesse)
- [Crypto-trading-bot](https://github.com/Haehnchen/crypto-trading-bot)
- [Wolf-bot](https://github.com/Ekliptor/WolfBot)
- [SuperAlgos](https://github.com/Superalgos/Superalgos)

### Quick comparison as of November 2020

|                        | Freqtrade                                                    | Zenbot                                                       | Jesse-AI                                                     | Crypto-trading-bot                                  | Wolf-bot                                                     | SuperAlgos                                                   |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| github stars           | 3.7 K                                                        | 6.8 K                                                        | 449                                                          | 608                                                 | 238                                                          | 66                                                           |
| Base code              | Python                                                       | Node.js                                                      | Python                                                       | Node.js                                             | TypeScript for Node.js                                       | Node.js                                                      |
| DDBB                   | sqlite3                                                      | Mongo.db                                                     | PostgreSQL                                                   | sqlite3                                             | Mongo.db                                                     | -                                                            |
| Notifications          | Telegram                                                     | Telegram, Pushbullet, Slack, XMPP, IFTT, DISCORD, Prowl, TextBelt, Adamant | Slack, Email                                                 | Slack, email, Telegram                              | Pushover                                                     | Telegram, Webhook                                            |
| Docs quality           | Superb                                                       | Superb                                                       | New - wip                                                    | Good                                                | good - technical                                             | New but evolving pretty quickly                              |
| Community support      | Reddit, Telegram, Slack                                      | Reddit, Telegram, Slack                                      | Dedicated forum                                              | Telegram                                            | Good                                                         | Telegram                                                     |
| Last updated           | 1 day                                                        | 2 days                                                       | 15 hours                                                     | 2 days                                              | last month                                                   | 4 days ago                                                   |
| Linux support          | YES                                                          | YES                                                          | YES                                                          | YES                                                 | YES                                                          | YES                                                          |
| Win/Mac support        | Partially under docker                                       | Under Docker (I couldn't make it work)                       | Native                                                       | ?                                                   | NO                                                           | NO                                                           |
| Docker support         | YES                                                          | YES                                                          | YES                                                          | YES                                                 | NO                                                           | NO                                                           |
| Backtest               | YES                                                          | YES                                                          | YES                                                          | YES                                                 | YES                                                          | YES                                                          |
| Paper trading          | YES                                                          | YES                                                          | YES                                                          | NO                                                  | NO                                                           | NO                                                           |
| Live trading           | YES                                                          | YES                                                          | NO                                                           | NO                                                  | YES                                                          | YES                                                          |
| Parameter optimization | YES                                                          | NO                                                           | YES                                                          | NO                                                  | WIP                                                          | NO                                                           |
| Machine learning       | NO                                                           | NO                                                           | NO                                                           | NO                                                  | WIP                                                          | NO                                                           |
| Supported exchanges    | Bittrex, Binance, Kraken, 113 others ongoing (CCXT maybe)    | Binance, Bitfinex, Bitstamp, Bittrex, CEX.IO, GDAX, Gemini, HitBTC, Kraken, Poloniex, QuadrigaCX and TheRockTrading, others ongoing. | Binance, Bitfinex, Coinbase (No live trading by the moment)  | Bitmex, Binance, Coinbase Pro, Bitfinex, Bybit, FTX | 30+ directly, over 130 using CCXT                            | CCXT                                                         |
| UI                     | Partially implemented in Jupyter notebooks with custom data plotting commands | Partially implemented with HTML outputs of tests and a basic live trade view | Partially implemented in Jupyter notebooks with custom data plotting commands | Webserver                                           | Webserver                                                    | Webserver                                                    |
| Other Notes            | No support for two open positions in the same pair           | I couldn´t make it work under Windows some months ago, maybe this could be already possible | New, mostly WIP but interesting set of features and under hard development | New, mostly WIP but interesting set of features     | Paid option with cloud service<br />WIP: Sentimental analysis, Tradingview integration | Visual Scripting Designer and debugger<br />Future paid subscription planned |
