---
title: "Market Type Trading Indictaor"
layout: post
date: 2023-01-23 22:10
tag: jekyll
image: https://github.com/mb0x88/mb0x88.github.io/blob/gh-pages/assets/images/market-type.png
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: "Market Type Indicator"
category: project
author: mattbell
externalLink: false
---

![Screenshot]([assets/images/market-type.png](https://github.com/mb0x88/mb0x88.github.io/blob/gh-pages/assets/images/market-type.png))

I was a big fan of [Van K Tharp](https://www.vantharp.com).  He talks in his books about defining a market type, so I built a indicator on Tradingview.

---

What has inside?

- Gulp
- BrowserSync

---

{% highlight js %}
// Sticky Header
//@version=4
study(title="Market Type", shorttitle="Market Type", overlay=false)

//resclose=security(syminfo.tickerid, res, close, lookahead=false)
//Dprinosc = resclose / resclose[1] - 1
//
Period = input(100, minval=5, title="SQN Period")
contracts = input(100, minval=0, type=input.float,title="Contracts")
account = input(60000, minval=100, type=input.float,title="Account Value")



period = input(title = "Display Period", type=input.string, defval="Current", options = ['Day', 'Week', 'Month', 'Current'])
//period = input(title = "Display Period", type=input.string, defval="Current", options = ['Current'])


res1 = period == 'Day' ? 'D' : period == 'Week' ? 'W' : period == 'Month' ? 'M' : period == 'Current' ? timeframe.period : 'D'

tfclose=security(syminfo.tickerid, res1, close,barmerge.lookahead_off)
bar_indexw=security(syminfo.tickerid, "W", bar_index, gaps=true)
bar_indexm=security(syminfo.tickerid, "M", bar_index, gaps=true)
bar_indexd=security(syminfo.tickerid, "D", bar_index, gaps=true)

sqnperiodm=bar_indexm<100 and bar_indexm>1?bar_indexm:Period



//plot(bar_indexm)
//
//


Dprinos = tfclose / tfclose[1] - 1

//xx = f_secureSecurity(syminfo.tickerid, "D", _Dprinos)

dailyclose = security(syminfo.tickerid, "D", close,barmerge.lookahead_off)
weeklyclose = security(syminfo.tickerid, "W", close,barmerge.lookahead_off)
monthlyclose = security(syminfo.tickerid, "M", close,barmerge.lookahead_off)


Dprinosd = dailyclose / dailyclose[1] - 1
Dprinosw = weeklyclose / weeklyclose[1] - 1
Dprinosm = monthlyclose / monthlyclose[1] - 1

Dprinosx = security(syminfo.tickerid, res1, Dprinos,barmerge.lookahead_off)

Stdevx = stdev(Dprinosx, Period)
Stdevxx = security(syminfo.tickerid, res1, Stdevx,barmerge.lookahead_off)
Dprosjekx = sma(Dprinosx, Period)
Dprosjekxx = security(syminfo.tickerid, res1, Dprosjekx,barmerge.lookahead_off)

SQNx = Dprosjekxx * sqrt(Period) / Stdevxx
cx = SQNx > 0.75 and SQNx <= 1.47 ? color.lime : 
   SQNx > -0.7 and SQNx < 0 ? color.red : SQNx > 1.47 ? color.green : 
   SQNx <= -0.7 ? color.maroon : SQNx >= 0 and SQNx <= 0.75 ? color.gray : na
plot(SQNx, title="SQNx", color=cx, transp=60, histbase=0, style=plot.style_area, linewidth=1)




Stdevd = stdev(Dprinosd, Period)
Stdevdd = security(syminfo.tickerid, "D", Stdevd,barmerge.lookahead_off)
Dprosjekd = sma(Dprinosd, Period)
Dprosjekdd = security(syminfo.tickerid, "D", Dprosjekd,barmerge.lookahead_off)


SQNd = Dprosjekdd * sqrt(Period) / Stdevdd
cd = SQNd > 0.75 and SQNd <= 1.47 ? color.lime : 
   SQNd > -0.7 and SQNd < 0 ? color.red : SQNd > 1.47 ? color.green : 
   SQNd <= -0.7 ? color.maroon : SQNd >= 0 and SQNd <= 0.75 ? color.gray : na
//plot(SQNd, title="SQNd", color=cd, transp=60, histbase=0, style=plot.style_area, linewidth=1)





Stdevw = stdev(Dprinosw, Period)
Stdevww = security(syminfo.tickerid, "W", Stdevw,barmerge.lookahead_off)
Dprosjekw = sma(Dprinosd, Period)
Dprosjekww = security(syminfo.tickerid, "W", Dprosjekw,barmerge.lookahead_off)


SQNw = Dprosjekww * sqrt(Period) / Stdevww
cw = SQNw > 0.75 and SQNw <= 1.47 ? color.lime : 
   SQNw > -0.7 and SQNw < 0 ? color.red : SQNw > 1.47 ? color.green : 
   SQNw <= -0.7 ? color.maroon : SQNw >= 0 and SQNw <= 0.75 ? color.gray : na
//plot(SQNw, title="SQNw", color=cw, transp=60, histbase=0, style=plot.style_area, linewidth=1)

//plot(bar_index)

Stdevm = stdev(Dprinosm, sqnperiodm)
Stdevmm = security(syminfo.tickerid, "M", Stdevm,barmerge.lookahead_off)
Dprosjekm = sma(Dprinosm, sqnperiodm)
Dprosjekmm = security(syminfo.tickerid, "M", Dprosjekm,barmerge.lookahead_off)

//plot(sqnperiodm)
SQNm = Dprosjekmm * sqrt(sqnperiodm) / Stdevmm
cm = SQNm > 0.7 and SQNm <= 1.47 ? color.lime : 
   SQNm > -0.7 and SQNm < 0 ? color.red : SQNm > 1.47 ? color.green : 
   SQNm <= -0.7 ? color.maroon : SQNm >= 0 and SQNm <= 0.75 ? color.gray : na
//plot(SQNm, title="SQNm", color=cm, transp=60, histbase=0, style=plot.style_area, linewidth=1)

length = input(10, minval=1)


annual = 365
per = timeframe.isintraday or timeframe.isdaily and timeframe.multiplier == 1 ? 1 : 7
hv = 100 * stdev(log(close / close[1]), length) * sqrt(annual / per)
hv2 = 100 * stdev(log(close / close[1]), length) * sqrt(annual / 7)

//plot(hv, color=color.blue)
pr=percentrank(hv,annual/per)
pr2=percentrank(hv2,12)
prw = security(syminfo.tickerid, "W", pr, lookahead=true)
prd = security(syminfo.tickerid, "D", pr, lookahead=true)
prm = security(syminfo.tickerid, "M", pr2, lookahead=true)

prx = security(syminfo.tickerid, res1, pr, lookahead=true)

prz=(prd-50)/25

przdisplay=(prx-50)/25

plot(przdisplay,color=color.white)
//plot(pr2_1,color=color.blue)
//plot(pr3_1,color=color.purple)
//plot(pr4_1,color=color.gray)

h1 =hline(-2)
h2 =hline(-1)
h3 =hline(0)
h4 =hline(1)
h5 =hline(2)

fill(h1,h2,color=prx<25?color.lime:na)
//fill(h1,h2,color=color.gray)
fill(h2,h3,color=prx>25 and prx<50?color.blue:na)
fill(h3,h4,color=prx>50 and prx<75?color.purple:na)
fill(h4,h5,color=prx>75?color.red:na)

//var sqnz = na
//SQN > 0.7 and SQN < 1.7?"bull":SQN > -0.7 and SQN < 0?"bear":SQN > 1.37?"strongbull":SQN < -0.7?"strongbear":SQN > 0 and SQN < 0.7 ?"neutral":na

//var przz = na
//prd<25?"quiet":prd>25 and prd<50?"normal":prd>50 and prd<75?"volatile":prd>75?"veryvolatile":na



//
string sqnfloatd = SQNd > 0.75 and SQNd < 1.47?" bull":SQNd > -0.7 and SQNd < 0?" bear":SQNd >= 1.47?" strong bull":SQNd <= -0.7?" strong bear":SQNd >= 0 and SQNd <= 0.75 ?" neutral":"[no data]"
string hvfloatd = prd<=25?" quiet":prd>25 and prd<50?" normal":prd>50 and prd<75?" volatile":prd>=75?" very volatile":"[no data]"


string sqnfloatw = SQNw > 0.75 and SQNw < 1.47?" bull":SQNw > -0.7 and SQNw < 0?" bear":SQNw >= 1.47?" strong bull":SQNw <= -0.7?" strong bear":SQNw >= 0 and SQNw <= 0.75 ?" neutral":"[no data]"
string hvfloatw = prw<=25?" quiet":prw>25 and prw<50?" normal":prw>50 and prw<75?" volatile":prw>=75?" very volatile":"[no data]"

string sqnfloatm = SQNm > 0.75 and SQNm < 1.47?" bull":SQNm > -0.7 and SQNm < 0?" bear":SQNm >= 1.47?" strong bull":SQNm <= -0.7?" strong bear":SQNm >= 0 and SQNm <= 0.75 ?" neutral":"[no data]"
string hvfloatm = prm<=25?" quiet":prm>25 and prm<50?" normal":prm>50 and prm<75?" volatile":prm>=75?" very volatile":"[no data]"

string sqnfloatdsimple = SQNd > 0.75 ?" up":SQNd < 0?" down":SQNd >= 0 and SQNd <= 0.75 ?" sideways":"[no data]"
string hvfloatdsimple = prd<=25?" quiet":prd>25 and prd<75?" normal":prd>=75?" volatile":"[no data]"

float contractvol = (contracts*atr(14))/account*100


string text1 = "                                                                     " + " daily:" + hvfloatd + sqnfloatd
string text2 = "                                                                                                                                                                                        " + " weekly:" + hvfloatw + sqnfloatw
string text3 = "                                                                                                                                                                                        " + " monthly:" + hvfloatm + sqnfloatm
string text4 = "                                                                     " + " simple:" + hvfloatdsimple + sqnfloatdsimple
string text5 = "                                                                     " + " positon volatility: " + tostring(round(contractvol,2)) + "%"

//
bool drawBar = barstate.islast
if (drawBar)
    label l1 = label.new(bar_index, 0.5, text1,  color=color.red, textcolor=color.black,  style=label.style_none,yloc=yloc.price,xloc = xloc.bar_index)
    label.delete(l1[1])
    label l2 = label.new(bar_index, 0, text2,  color=color.red, textcolor=color.black,  style=label.style_none,yloc=yloc.price,xloc = xloc.bar_index)
    label.delete(l2[1])
    label l3 = label.new(bar_index, -1.5, text3,  color=color.red, textcolor=color.black,  style=label.style_none,yloc=yloc.price,xloc = xloc.bar_index)
    label.delete(l3[1])
    label l4 = label.new(bar_index, -1.5, text4,  color=color.red, textcolor=color.black,  style=label.style_none,yloc=yloc.price,xloc = xloc.bar_index)
    label.delete(l4[1])
    label l5 = label.new(bar_index, 2, text5,  color=color.red, textcolor=color.black,  style=label.style_none,yloc=yloc.price,xloc = xloc.bar_index)
    label.delete(l5[1])
//    label l5 = label.new(bar_index, -22, text5,  color=color.red, textcolor=color.black,  style=label.style_none,yloc=yloc.price)
//
{% endhighlight %}
