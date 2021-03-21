---
layout: post
title:      "Reading csv file With JavaScript"
date:       2021-03-21 21:13:11 +0000
permalink:  reading_csv_file_with_javascript
---


In this post I am going to share a skill I recently learned while completing a challenge. Up until now, I have never worked on a project that required me to read data from a csv file instead of a database with JavaScript. Here I am going to share one approach to do so using HTML5 and `Papa parse library`.

`Papa parse` is a JavaScript library for fast 'in-browser' CSV parsing. It is known to be very reliable and easy in parsing CSV files as it provides more advance features or options. It is supported by most modern browsers. You can check [Papa parse documention](https://www.papaparse.com/docs) to learn all it features.

The approach I used involves first uploading the csv file using HTML5 `<input type="file">` element, then use `Papa parse` to parse the file that can be used for further processing like sending it to the server to store the data in a database or display the data in a table. 

## Step 1

create an html file and include the Papa parse file in the head section. You can name your file `index.html` or whatever you want.

```html

<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title>CSV File upload and Parsing</title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="">
        <!-- Papa parse min js -->
        <script src="https://unpkg.com/papaparse@latest/papaparse.min.js"></script>
    </head>
    <body>
       
    </body>
</html>
```

## Step 2
Add a html form to upload your csv formatted file to `index.html`

```html

<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title>CSV File upload and Parsing</title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="">
				<!-- Papa parse min js -->
        <script src="https://unpkg.com/papaparse@latest/papaparse.min.js"></script>
    </head>
    <body>
        <form>
            <div>
                <label for="files">Upload your csv file:</label>
                <input type="file" id="files"   accept=".csv" required />
            </div>
            <div>
                <button type="submit" id="submit-file" >Upload File</button>
            </div>
        </form>
		<!-- your js files goes here -->
		<script src="script.js" async defer></script>
    </body>
</html>
```

As seen in the above code, I added some validation to the input file, adding the `required` attribute and accepting only file with `.csv` extension.

### Output in-browser
![form image in-browser](https://lh4.googleusercontent.com/iWOGxRv4_vO39vSRH_jH-oTJydUhiuUn8uWoPAEIprq6aDPGImIyvxB9jSOrXmDZyfuJpbp0F8GWF0_2iX1Tm19kvpT4fDhSpQKuAGs)

## Step 3
create a js file `script.js` that will have the code logic to parse the csv data using `Papa.parse` method. The method takes the `file object` and a `config object` as arguments. The config object defines settings, behavior, and callbacks used during parsing. Below are the default properties of the config object:

```js

{
	delimiter: "",	// auto-detect
	newline: "",	// auto-detect
	quoteChar: '"',
	escapeChar: '"',
	header: false,
	transformHeader: undefined,
	dynamicTyping: false,
	preview: 0,
	encoding: "",
	worker: false,
	comments: false,
	step: undefined,
	complete: undefined,
	error: undefined,
	download: false,
	downloadRequestHeaders: undefined,
	downloadRequestBody: undefined,
	skipEmptyLines: false,
	chunk: undefined,
	chunkSize: undefined,
	fastMode: undefined,
	beforeFirstChunk: undefined,
	withCredentials: undefined,
	transform: undefined,
	delimitersToGuess: [',', '\t', '|', ';', Papa.RECORD_SEP, Papa.UNIT_SEP]
}
```

You can find the description for each property [here](https://www.papaparse.com/docs#config).
In this post, we are only going to use two properties: the `header` which we will set to `true` to interpret the first row of the parsed data as the property or field names, and each row of the csv file will be an object of values keyed by field name.
The second property we will use here is `complete` which is the callback function to execute when parsing is complete. The callback takes the parsed results as an argument.

`script.js`

```js

// Add event listener to form to read file once submitted
const form = document.querySelector("form");

form.addEventListener("submit", e => {
    e.preventDefault();
    // save the file from the input file
    const file = e.target[0].files[0]; // getting the first input of the form then the first file of its files property (array)

    //parse the file with Papa.parse
    Papa.parse(file, {
        header: true,
        complete: function(results) {
            // read the data from results
            const { data } = results;
            //print the data in the console
            console.log({ data })
        }
    });
    form.reset();
})
```

The above code in the `script.js` file will execute once the user submit the its csv file.

Now we have a program to parse csv files. Once the file is parsed, you can manipulate the data however you want to meet your project's requirement.  As a demo, let's parse the following csv file `sample.csv`

![sample.csv file](https://lh6.googleusercontent.com/BXBHWQaVP4vf7EvUMw0bu8AxQ4LtBTGTK0mZmgZH9oqwFZvTuTsE0Jk1wLCooStAxpgufx9wpal4PHWCGBRbZXD_rRoKboKAC-1QPFvvugBRswq141wCRwy6FX2irDVR8z5ewwl5)

Here is the parsed data structure after running the program :

![parsed data](https://lh4.googleusercontent.com/wdCcmmLsx6tuZCeq7zagBlfVSPjF6BmNn2C_PYA26v6Peila8a5aRhzA6-Gaf5vw7xiBA81oqV5Y4NyWc4frMN86ftRtPbbZV8xVFKjN9B3JQPpvQKoe55EMC-zFWs1-P4ofnL4l)

I hope you find this post useful.

Until next time,

Happy coding/learning !!
