---
layout: post
title:      "Weather-Forecast Ruby CLI Gem"
date:       2020-03-16 00:57:52 +0000
permalink:  weather-forecast_ruby_cli_gem
---



This week I completed my first CLI project using ruby as tasked by Flatironschool software engineering program. The project requires us to build a CLI that provides access to data from the web, collect the data and save it into an object. We were also required to go at least one level deep.
After wandering on the web for ideas and looking for good free API to collect the data from, I decided to build a simple Weather gem because Who doesn’t like a forecast update?


Well at least, that the first thing I do before getting out, CHECK THE WEATHER!!
Working with API for the first time was quite a challenge. Geez… the amount of time I spent on the web to teach myself.
Check My code here: https://github.com/vanessuniq/weather-forecast

**App Description:**

My app allows user to get a weather update by zip code. It provides a 24 hour forecast, detailing it every 3 hours. I used the OpenWeatherMap API to collect my data, which was returned in a JSON format. It was not difficult to work with JSON as I know how to operate on hashes or array with Ruby.

**My work Structure is as follow:**
Fetching data from the Web using OpenWeatherMap API
The app prompt the user to enter a zip code. Once a valid zip code is retrieved, the app used HTTParty gem to query the web and JSON parser to parse the response which will then be used to provides user requested data. The easiest way to learn how a gem works is by reading the README. It literally tells you how to use the gem. Well, you can always google everything…, But seriously, go to the source
 
 

**Collecting Data into Objects**
Next I used the skills learned during this first part of the program at Flatironschool to collect data into objects. The response from the API provided 40 data points enclosed in an array. The data points in the main array were either hashes or arrays. Those data points represent weather data every 3 hours for 5 days.  I used iteration to collect the details I needed for my app, as well as the built in Ruby class Struct . A struct is useful in terms of producing value objects which store related attributed together. 
 

Now I had an array of Forecast containing the details I need. 
Next, I decided to display forecast to user in a 24hrs period. So I had to divide my array into 5 arrays, each containing 8 elements (8 weather data) that would be display to user per request. I used the terminal-table gem to display the data.
 


 
Ouf! The hard work was done. 

<div style="width:100%;height:0;padding-bottom:46%;position:relative;"><iframe src="https://giphy.com/embed/3ohzdIuqJoo8QdKlnW" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/reactionseditor-yes-awesome-3ohzdIuqJoo8QdKlnW">via GIPHY</a></p>

All I had to do at this point was to transfer the collected data into my CLI, and used some styling gem like Colorize or Figlet  to improve the user experience. 
Although I am happy with the end result, This CLI can be further refactored.  

**My TO DO List:**

1.	Hide my API keys
2.	Possibly add some emoji and more style
3.	Revise hard coding

Thanks for reading, until next time…
Let’s break in!

