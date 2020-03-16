---
layout: post
title:      "Weather-Forecast Ruby CLI Gem"
date:       2020-03-16 00:57:52 +0000
permalink:  weather-forecast_ruby_cli_gem
---

This week I completed my first CLI project using ruby as tasked by Flatironschool software engineering program. The project requires us to build a CLI that provides access to data from the web, collect the data and save it into an object. We were also required to go at least one level deep.
After wandering on the web for ideas and looking for good free API to collect the data from, I decided to build a simple Weather gem because Who doesn’t like a forecast update?

<div style="width:100%;height:0;padding-bottom:100%;position:relative;"><iframe src="https://giphy.com/embed/RMefA2gSRVtC5P2XVN" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/wdr-denken-glanzundnatur-glanznatur-RMefA2gSRVtC5P2XVN"></a></p>

Well at least, that is the first thing I do before getting out, CHECK THE WEATHER!!
Working with API for the first time was quite a challenge. Geez… the amount of time I spent on the web to teach myself.

Well, it Paid off!!

 <button><a href="https://github.com/vanessuniq/weather-forecast">Check my code here:</a></button>

**App Description**

My app allows user to get a weather update by zip code. It provides a 24 hour forecast, detailing it every 3 hours. I used the OpenWeatherMap API to collect my data, which was returned in a JSON format. It was not difficult to work with JSON as I know how to operate on hashes or array with Ruby.

**Project Outline / Step**

**Fetching data from the Web using OpenWeatherMap API**

The app prompt the user to enter a zip code. Once a valid zip code is retrieved, the app used HTTParty gem to query the web and JSON parser to parse the response which will then be used to provides user requested data. The easiest way to learn how a gem works is by reading the README. It literally tells you how to use the gem. Well, you can always google everything…, But seriously, go to the source
 
 ![image](https://user-images.githubusercontent.com/46642178/76722865-c4196d00-671b-11ea-963c-66073eef4c61.png)

**Collecting Data into Objects**

Next I used the skills learned during this first part of the program at Flatironschool to collect data into objects. The response from the API provided 40 data points enclosed in an array. The data points in the main array were either hashes or arrays. Those data points represent weather data every 3 hours for 5 days.  I used iteration to collect the details I needed for my app, as well as the built in Ruby class Struct . A struct is useful in terms of producing value objects which store related attributes together. 
 
![image](https://user-images.githubusercontent.com/46642178/76723090-7a7d5200-671c-11ea-9c67-a36afdaeb71b.png)

Now I had an array of Forecast containing the details I need. 
Next, I decided to display forecast to user in a 24hrs period. So I had to divide my array into 5 arrays, each containing 8 elements (8 weather data) that would be display to user per request. I used the terminal-table gem to display the data.

 
![image](https://user-images.githubusercontent.com/46642178/76723170-c4fece80-671c-11ea-9758-75e1d8120e80.png)


![image](https://user-images.githubusercontent.com/46642178/76723718-9c77d400-671e-11ea-9f8e-ca6ca1d9a891.png)

 
Ouf! The hard work was done. 

<div style="width:100%;height:0;padding-bottom:46%;position:relative;"><iframe src="https://giphy.com/embed/3ohzdIuqJoo8QdKlnW" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/reactionseditor-yes-awesome-3ohzdIuqJoo8QdKlnW"></a></p>

**CLI Time**

All I had to do at this point was to transfer the collected data into my CLI, and used some styling gem like Colorize, TTY or ruby_Figlet  to improve the user experience. 
Upon execution, the app starts with a banner 'Weather-Forecast' that was obtained using the ruby_figlet gem. This is a useful and simple ruby gem for font interpretation and printing. You can check the installation and usage of any these useful gems on their github repository, checking the README file.  This gem has many font I encourageto checkout for your CLI. I used the simple one shown bellow:

![image](https://user-images.githubusercontent.com/46642178/76725595-1743ed80-6725-11ea-9a88-5706cd099784.png)

![image](https://user-images.githubusercontent.com/46642178/76725452-a0a6f000-6724-11ea-96ac-31270b0db09e.png)

The code above also display how one can use the colorize gem.

Fnally, my last gem.. The one I discover last and I told myself I GOT TO USE THIS ONE! The TTY-PROMPT

We all know the drill of having to throw off a bunch of if/else statements to prevent the user from inputing a wrong entry for a selection menu. This becomes annoying when you have you have a long selection or plan to expand the options.

Well, I thank the creator of TTY-Prompt. This is a life savior. This gem improves you menu building capabilities. It not only refactors your code, but also makes it interactive.

<a href="https://github.com/piotrmurach/tty-prompt/blob/master/README.md#261-choices" target="_BLANK">Check the gem README page</a> on github for more gem usage other than the menu selection.

I use the following code for my menu:

prompt = TTY::Prompt.new
@options = %w(Exit Check-Another-Area Next-Day-Forecast Next-2-Days-Forecast Next-3-Days-Forecast Next-4-Days-Forecast)</strong></h5>
@selection = prompt.select("What would you like to do next?, @options)

  After creating an instance of prompt, we use the select method on the prompt and passig it 2 arguments: the question to be asked and the list of options. and our result from the prompt: 
  
  ![image](https://user-images.githubusercontent.com/46642178/76728350-d8199a80-672c-11ea-96b2-c559d00f7157.png)
  

Although I am happy with the end result, my project can be further refactored.  

**My TO DO List:**

1.	Hide my API keys
2.	Possibly add some emoji and more style
3.	Revise any hard coding

Thanks for reading, until next time…

Let’s break in!

