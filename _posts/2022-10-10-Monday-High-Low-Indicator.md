---
title: "Monday Highh & Low Indicator"
layout: post
date: 2022-10-10 22:10
tag: jekyll
image: /assets/images/monday.png
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: "Monday Highh & Low Indicator"
category: project
author: mattbell
externalLink: false
---

![Screenshot](/assets/images/monday.png)

Here is an indicator I built that displays the Monday high and low for the week.




---

{% highlight js %}



// Description:  plot lines onto Monday High/Low

//@version=4
study(title="Monday High + Low", overlay=true)
show_labels_1 = input(true,title="Show Monday Range Label?")
show_lines_1 = input(true,title="Show Monday Range Lines?")
show_prev = input(false, title="Show Previous Monday Range?")
show_current = input(true, title="Show Current Monday Range?")
res1 = "D"// input (type=input.resolution, defval="D", title="Open TF 1")
res1_ago = 0 //input (defval=1)
//displaced_space1 = input(defval="                                                                                                                                                        ", title="Spacing")

//user_color = input(title="Color", defval="black", options=["aqua", "black", "blue", "fuchsia", "gray", "green", "lime", "maroon", "navy", "olive", "orange", "purple", "red", "silver", "teal", "white", "yellow"]) 
//final_color = user_color == "aqua" ? color.aqua : user_color == "black" ? color.black : user_color == "blue" ? color.blue : user_color == "fuchsia" ? color.fuchsia : user_color == "gray" ? color.gray : user_color == "green" ? color.green : user_color == "lime" ? color.lime : user_color == "maroon" ? color.maroon : user_color == "navy" ? color.navy : user_color == "olive" ? color.olive : user_color == "orange" ? color.orange : user_color == "purple" ? color.purple : user_color == "red" ? color.red : user_color == "silver" ? color.silver : user_color == "teal" ? color.teal : user_color == "white" ? color.white : user_color == "yellow" ? color.yellow : color.black

color mondayrange = input(color.blue, "Monday Range", type = input.color)
color prevmondayrange = input(color.fuchsia, "Previous Monday Range", type = input.color)

time1= security(syminfo.ticker, res1, time[res1_ago], lookahead=barmerge.lookahead_on)

thisweek = security(syminfo.tickerid, "W", barstate.islast, lookahead = barmerge.lookahead_on)
firstday = security(syminfo.tickerid, "D", barstate.isfirst, lookahead = barmerge.lookahead_on)
new = security(syminfo.tickerid, "D", barstate.isnew, lookahead = barmerge.lookahead_on)

var index = 0
if thisweek
    index := bar_index
    
is_newbar(res) =>
    t = time(res)
    change(t) != 0 ? 1 : 0
    
    
firstcandle=is_newbar("D") and is_newbar("W")

//plotshape(firstcandle)


//fctime=time[firstcandle]

var countD = 0
countD := is_newbar("W") ? 1 : is_newbar("D") ? countD + 1 : countD



var barsback = 0
if countD[1]==5
    barsback := 5
    

x = countD[1] == 1
t = timestamp(year, month, dayofmonth, hour, minute)
var tAtx = 0
if x
    tAtx := t
    

firstcandlestart = firstcandle
tcandle = timestamp(year, month, dayofmonth, hour, minute)
var tcandlestart = 0
if firstcandlestart
    tcandlestart := tcandle + 86400000
    

isfirstcandle=timenow <= tcandlestart
//plotshape(isfirstcandle)
//plot(tcandle)
//plot(tcandlestart,color=color.red)
    

time2= security(syminfo.ticker, "D", time[6], lookahead=barmerge.lookahead_on)
time3= security(syminfo.ticker, "D", time[7], lookahead=barmerge.lookahead_on)


timeframe=timeframe.isdaily

is_monday = countD==1
is_notmonday = countD!=1

//plotshape(is_monday)
//plotshape(is_notmonday,color=color.red,location=location.belowbar)
//plot(countD)

float res1_price_H = 0.0 
res1_price_H := is_monday ? security(syminfo.prefix+":"+ syminfo.ticker, res1, high[res1_ago], lookahead=barmerge.lookahead_on) : res1_price_H[1]
float res1_price_L = 0.0 
res1_price_L := is_monday ? security(syminfo.prefix+":"+ syminfo.ticker, res1, low[res1_ago],  lookahead=barmerge.lookahead_on) : res1_price_L[1]
float res1_price_HL = 0.0 
res1_price_HL := is_monday ? security(syminfo.prefix+":"+ syminfo.ticker, res1, hl2[res1_ago],  lookahead=barmerge.lookahead_on) : res1_price_HL[1]

float res1_price_H_prev = 0.0 
res1_price_H_prev := is_monday ? res1_price_H[1]:na
float res1_price_L_prev = 0.0 
res1_price_L_prev := is_monday ? res1_price_L[1]:na
float res1_price_HL_prev = 0.0 
res1_price_HL_prev := is_monday ? res1_price_HL[1]:na


string text_H = "m high: "+ tostring(res1_price_H)
string text_L = "m low: "+ tostring(res1_price_L)
string text_HL = "m mid: "+ tostring(res1_price_HL)

string text_H_prev = "m high: "+ tostring(res1_price_H_prev)
string text_L_prev = "m low: "+ tostring(res1_price_L_prev)
string text_HL_prev = "m mid: "+ tostring(res1_price_HL_prev)

isnotMonday=countD==2 or countD==3 or countD==4 or countD==5 or countD==6 or countD==7

//plotshape(new and thisweek and is_monday)

//and isnotMonday()
//bool drawBar = barstate.islast and isnotMonday



if (is_monday and show_labels_1 and show_current and timeframe.isintraday)
    label l1 = label.new(time1, res1_price_H, text_H,  color=color.blue, textcolor=color.blue, style=label.style_none,yloc=yloc.price,xloc=xloc.bar_time)
    label.delete(l1[1])
    if isfirstcandle==true
        label.delete(l1[0]) 
    label l2 = label.new(time1, res1_price_L, text_L,  color=color.blue, textcolor=color.blue, style=label.style_none,yloc=yloc.price,xloc=xloc.bar_time)
    label.delete(l2[1])
    if isfirstcandle==true
        label.delete(l2[0]) 
    label l3 = label.new(time1, res1_price_HL, text_HL,  color=color.blue, textcolor=color.blue, style=label.style_none,yloc=yloc.price,xloc=xloc.bar_time)
    label.delete(l3[1])
    if isfirstcandle==true
        label.delete(l3[0]) 


//plotshape(isfirstcandle)
//plotshape(isnotMonday)


if is_monday and show_lines_1 and show_current and timeframe.isintraday
    line line_1 = line.new(time1, res1_price_H, time +60*60*24, res1_price_H, xloc=xloc.bar_time, extend=extend.right,style=line.style_dashed,color=mondayrange, width=2)
    line.delete(line_1[1])
    if isfirstcandle==true
        line.delete(line_1[0])   
    line line_2 = line.new(time1, res1_price_L, time +60*60*24, res1_price_L, xloc=xloc.bar_time, extend=extend.right, style=line.style_dashed,color=mondayrange, width=2)
    line.delete(line_2[1])
    if isfirstcandle==true
        line.delete(line_2[0])   
    line line_3 = line.new(time1, res1_price_HL, time +60*60*24, res1_price_HL, xloc=xloc.bar_time, extend=extend.right, style=line.style_dotted,color=mondayrange, width=2)
    line.delete(line_3[1]) 
    if isfirstcandle==true
        line.delete(line_3[0])   
    
if is_monday and show_lines_1 and isfirstcandle and timeframe.isintraday
    line line_11 = line.new(tAtx, res1_price_H_prev, time +60*60*24, res1_price_H_prev, xloc=xloc.bar_time, extend=extend.right,style=line.style_dashed,color=prevmondayrange, width=2)
    line.delete(line_11[1])
//    if isfirstcandle!=true
//        line.delete(line_11[0])
    line line_22 = line.new(tAtx, res1_price_L_prev, time +60*60*24, res1_price_L_prev, xloc=xloc.bar_time, extend=extend.right, style=line.style_dashed,color=prevmondayrange, width=2)
    line.delete(line_22[1])
//    if isfirstcandle!=true
//        line.delete(line_22[0])
    line line_33 = line.new(tAtx, res1_price_HL_prev, time +60*60*24, res1_price_HL_prev, xloc=xloc.bar_time, extend=extend.right, style=line.style_dotted,color=prevmondayrange, width=2)
    line.delete(line_33[1])  
//    if isfirstcandle!=true
//        line.delete(line_33[0])


if (is_monday and show_labels_1 and isfirstcandle and timeframe.isintraday)
    label l11 = label.new(tAtx, res1_price_H_prev, text_H_prev,  color=prevmondayrange, textcolor=prevmondayrange, style=label.style_none,yloc=yloc.price,xloc=xloc.bar_time)
    label.delete(l11[1])
    if isfirstcandle!=true
        label.delete(l11[0]) 
    label l22 = label.new(tAtx, res1_price_L_prev, text_L_prev,  color=prevmondayrange, textcolor=prevmondayrange, style=label.style_none,yloc=yloc.price,xloc=xloc.bar_time)
    label.delete(l22[1])
    if isfirstcandle!=true
        label.delete(l22[0]) 
    label l33 = label.new(tAtx, res1_price_HL_prev, text_HL_prev,  color=prevmondayrange, textcolor=prevmondayrange, style=label.style_none,yloc=yloc.price,xloc=xloc.bar_time)
    label.delete(l33[1])
    if isfirstcandle!=true
        label.delete(l33[0]) 

if is_monday and show_lines_1 and show_prev and timeframe.isintraday
    line line_11 = line.new(tAtx, res1_price_H_prev, time +60*60*24, res1_price_H_prev, xloc=xloc.bar_time, extend=extend.right,style=line.style_dashed,color=prevmondayrange, width=2)
    line.delete(line_11[1])
//    if isfirstcandle!=true
//        line.delete(line_11[0])
    line line_22 = line.new(tAtx, res1_price_L_prev, time +60*60*24, res1_price_L_prev, xloc=xloc.bar_time, extend=extend.right, style=line.style_dashed,color=prevmondayrange, width=2)
    line.delete(line_22[1])
//    if isfirstcandle!=true
//        line.delete(line_22[0])
    line line_33 = line.new(tAtx, res1_price_HL_prev, time +60*60*24, res1_price_HL_prev, xloc=xloc.bar_time, extend=extend.right, style=line.style_dotted,color=prevmondayrange, width=2)
    line.delete(line_33[1])  
{% endhighlight %}
