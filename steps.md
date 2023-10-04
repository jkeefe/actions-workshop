
# Workshop at the Buenos Aires Media Party, October 2023

## Setup: Github Codespaces

We'll get the code and get started with Github Codespaces

### References

- https://docs.github.com/en/codespaces/getting-started/quickstart
- Pricing: https://docs.github.com/en/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces

### Steps

- working on a computer in the cloud ... it's called a codespace

- go to my repo: https://github.com/jkeefe/actions-workshop
- fork
- code > Codespaces > Create Codespace on Main

- wait a little bit for it to get started
- should have a full screen
- quick tour:
    - big top box is where we'll write our code
    - the big bottom box is where we'll run things
    - the left side panel has our tools and files
        - menu
        - files
        - search
        - source control
            - we're going to save all of this code into your own account
            - click on the "source control" icon
            - type a commit messaage, not to yourself, usually starting with a verb: "completed the setup"
            - "publish branch"
            - note that it has _your_ account number in the options here
            - use the public one ... we're not doing anything secret, and private repos have limitations
            - (I have to add a 2 at the end, because I already have a repo called knightcenter-data-mapping)
            - down in the corner, you can view your code on Github ... do that!
            - This is YOUR COPY of the code!
            - Close this tab.
        - return to the files explorer

- Open a new tab
- Go to (github.com/codespaces)[https://github.com/codespaces]
- Remember I said this was a computer in the cloud? Here's where that computer is listed
- Let's do a few things here:
    - Go to the three dots in the first entry here:
    - Rename it from the silly name to: "My class codespace"
    - Next, click on "Keep codespace" ... Github will delete it automatically after 30 days, but we want to save it
    - Finally ... 
        - Running computers cost money
        - You get 60 hours free every month and 15 gigabytes of storage ... which will be more than enough
        - But let's not waste those free hours
        - So "Stop codespace"
        - If you forget, don't worry: It'll shut down automatically after 30 minutes. But why waste that?

_When you're done for the day ..._

- Go to (github.com/codespaces)[https://github.com/codespaces]
- Click on the three dots in the row for "My class codespace"
- Click "Stop codespace"

_Returning to work_

- Go to (github.com/codespaces)[https://github.com/codespaces]
- Click _right on the title:_ "My class codespace"


## Explore the Makefile & the command line

- Put this into the Makefile:

```makefile
greeting:
	echo 'hello'
```

- Type `make greeting`

- Put this into the Makefile:

```makefile
math:
	expr 2 + 2
```

- Type `make math`

- Add this to the Makefile:

```makefile
directories:
	-mkdir tmp
	-mkdir data
```

## The "End Product": A Datawrapper Map

- Map: https://app.datawrapper.de/map/xlpOJ/publish
- See "Add Your Data" tab

## The Data Source: IMF's API

- Reference links:
    - IMF Inflation by country: https://www.imf.org/external/datamapper/PCPIPCH@WEO/WEOWORLD?year=2023
    - IMF api: https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023 
    - API reference: https://www.imf.org/external/datamapper/api/help 
    - Countries: https://www.imf.org/external/datamapper/api/v1/countries
    - Indicators/source: https://www.imf.org/external/datamapper/api/v1/indicators 

- Our key links:
    - https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023
    - https://www.imf.org/external/datamapper/api/v1/countries

- So to quickly make this map, I'd just download those two files
- Use Excel, Google sheets, or some other way to merge these files
- I wrote a script `imf_to_csv.js`
- Point is, something has to be *done* to this data to map it
- Remember, I probably went to do this every month. Will I remember? 
- I can essentially do the "notes" for this as a Makefile

- Do `curl "https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023"`
- Do `curl "https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023" -o tmp/inflation.json`

## Runable notes

(Could do this in any way you like. Node, python, bash, whatever)

Tip: See `.prebaked/makefile`

```makefile
downloads:
	curl "https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023" -o tmp/inflation.json
	curl "https://www.imf.org/external/datamapper/api/v1/countries" -o tmp/countries.json

downloads:
	curl "https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023" -o tmp/inflation.json
	curl "https://www.imf.org/external/datamapper/api/v1/countries" -o tmp/countries.json

freshdata:
	node imf_to_csv.js

all: directories downloads freshdata
```




________

## Module 1: Making maps with Datawrapper using external data

We'll kick off the class by making colorful, website-ready data maps with Datawrapper — and in the process, learn the steps necessary to link those maps to external data sources.

This module will cover:

- The essentials for preparing your data to map
- Building shape maps (choropleth maps) in Datawrapper
- Using external data to drive your maps
- Sharing maps with colleagues and publishing them online

**NOTES**

- IMF Inflation by country: https://www.imf.org/external/datamapper/PCPIPCH@WEO/WEOWORLD?year=2023
- IMF api: https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023 
- API reference: https://www.imf.org/external/datamapper/api/help 
- Countries: https://www.imf.org/external/datamapper/api/v1/countries
- Indicators/source: https://www.imf.org/external/datamapper/api/v1/indicators 

### Video 1-1

**INTRO**

- Hello! I'm so glad you're here. 
- In this video, we're going to jump right in and make a map.
- This is a choropleth map ... you know, the kind where we color in shapes based on data.
- To make this map, we need to touch on a key concept: Joining data & shapes 

[shapes + data talk / slides]

Make point of noting the ISO country codes!

Next up, we'll use real shapes and real data to make our map. 

### Video 1-2

**INTRO**

OK, now let's build a map. In this case, our assignment is to chart the inflation rates across latin america. First we need the data.

**SCREEN**

- In a browser, go to `bit.ly/knight-data` [done]
- Save to your downloads folder
- Open that with a text editor or spreadsheet program. If you don't have one, you can open a new tab and go to sheets.new and upload it to Google sheets.
- I'm going to use a text editor
- Note that we have the country name, and the ISO code and the inflation rate
- Remember, we're going to need to match this data to shapes of the countries
- And the shapes have to have a similar column
- We could go hunting for shape files of countries, but fortunately our mapping program has those already ... so ...
- Open a new tab
- Log into your datawrapper account at `datawrapper.de`, using the account information you gave in the introductory video
- Create new map
- Choropleth map
- And let's pick our shapes
- Datawrapper has lots of shapes, including all of the countries of the world. Excellent.
- Let's use the "Latin America" map.
- OK ... shapes, check!
- Let's add the data
- Then upload it to datawrapper
- It's a little confused!
- The thing we're going to match on — to join the shapes and the data — are the countries' 3-letter ISO code
- Better than guessing at the spelling or abbreviation of the name, right? For example, St. Vincent and the Grenadines could be represented with St. for saint, etc.
- Go to the match tab, and change the matching key to ISO-codes, which is what these are
- Ah! It's happier
- Let's look at the "Check" tab ...
    - Many of the countries aren't on the map! Like CAN
    - Some that dont match aren't actually countries, like EU
    - And the missing ones are missing data, like Cuba
- Hit "Proceed"
- You can see Datawrapper is already guessing at how we want our map to look!

***OUTRO***

Next up, we'll style our map and get it ready to publish.


### Video 1-3

**INTRO**

Now that we've joined our data to the country shapes in Datawrapper, it's time to use some of the styling and visualization features to finish the map.

**SCREEN**

- We're back at datawrapper, `datawrapper.de`
- We can see on the data page that datawrapper is already giving us a preview of the data.
- Let's make it better with the Visualize tab
- I think we can do better showing the values on the map using a stepped chart
- And then the "natural" or Jenks breaks, which takes into account the distribution of the data (venezuela is way high)
    - Side note ... if you'd like to dive deeper into how to pick breaks 
- Even better with 6 steps
- Title: Tracking inflation rates
- Legend: 
    - Change the format to percentages
    - Advanced > Size ~215px
    - Check how it looks on mobile
- Annotate tab:
    - Source
    - Note: Annual percent change in average consumer prices, as of April 2023
    - Could add a note, byline, etc.
    - Tooltip: 
        - Put the country in the top
        - Add the %

- Publish
    - Links and embed code for dropping into your story or CMS

**OUTRO**

Something about having this all set up and ready to go. Share one more trick nexxt time.


## Video 1-4

**INTRO**

OK, I want to show you one more cool trick that is going to demonstrate the power of what we'll learn the rest of the class. 

**SCREEN**

- Log into Datawrapper
- Open the map we made
- Click to step 2 - "Add your data"
- Instead of downloading and uploading our data ... let's just link to the data directly
- Pick: Link to external data set
- Pick: Serve data file directly
- put in URL of the original file ... https://bit.ly/knight-data
- OK, so we saved a step!

- 

**OUTRO**

Next week, I'll show you how I made that csv file ... and how to get it all prepared with a single command.

(FURTHER READING)

If you become a regular user of Datawrapper, they have a few way to prepare external data for their maps and charts. You can read about them at `bit.ly/datawrapper-external`.

The easier ways are specific to Datawrapper. I'm going to show the more "advanced" method ... because that's more universal, and will allow you to upload and use data files for any applications you might use — or build. 


## Module 2: Fetching and preparing your data with a single command using Makefiles

In our second week, we'll explore how to use a Makefile to handle tedious data-fetching and data-processing tasks ... and perform all of those duties with a single command.

- This module will cover:
- An introduction to the power of Makefiles
- Fetching your data
- Processing your data with code
- Tying it all together with one command: "make all"


**NOTES**

### Video 2-1

**PREP**

- Download the imf_to_csv file from this repo. 
- Make sure it works.
- site: https://www.imf.org/external/datamapper/PCPIPCH@WEO/OEMDC/ADVEC/WEOWORLD
- API: https://www.imf.org/external/datamapper/api/help 
- Data: https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023
- Countries: https://www.imf.org/external/datamapper/api/v1/countries

**INTRO**

Now I want to show you how I made that handy-dandy data file we used to make our inflation map. You can just watch this one, without doing anything hands-on or taking notes ... but just get a sense of the steps I had to go through to make the file we used; they'll come up again next week.

**SCREEN**

- Remember, this is how the CSV looks:
    - Show in Code
- Let's look at how I made this ...
- Our source for the data is the international monetary fund
    - Show that page
- I looked and they have an API, which led me to this file:
    - Data tab
- And also this file:
    - Countries
- Which looks like this:
    - This is a JSON file, and I use a plug-in called "JSON viewer" so I can see it prettier in my browser.
    - It's all the same data, just not mushed togeher
- So I'm going to download this ... right-click or contro-click on this page and save it to my Downloads folder as inflation.json
- I'm going to save countries, too, as countries.json

- Now how to merge these? And make them a CSV?

**FACE**

This is the point where how you process the data will depend on the data you have, and the format it's in.

You might want use 
    - csvkit ... to manage and process csvs `csvkit.readthedocs.io/`
    - jq ... to manage json files `jqlang.github.io/jq/`
    - mapshaper ... to handle geospatial data `bit.ly/mapshaper-commands`

All of these tools you can use on the **command line** in your terminal. That's key, if you can do it with typing — and not pointing and clicking with your mouse — you can teach a computer how to do it. They can type commands, and aren't so great at using a mouse.

**BACK TO SCREEN**

- In our case, we're going to use a little script I wrote: `imf_to_csv`
- It takes these two files I downloaded and turns them into the CSV we need.
- You have a copy of the code in your codespace
- If I run it ... 
- I get the CSV I need!


Again, you actually have a copy of this script in your Codespace. We'll use it next week, when we start automating our data collection!

**OUTRO**

Now, depending on your coding experience, you might not have the skills to write that little translation script. But there are often command-line tools you could use to process data.

We'll talk more about those soon ... but the point here is that by investing a little time in scripting your data procssing like this, you'll save time next time you need to make the same map ... and you can have a computer do it for you instead!


### Video 2-2

**INTRO**

Welcome back.

So let's say it's several months later, and you want to update the map with the latest inflation numbers. You might have to go back and remember how you did it. Maybe you even made some notes ...

- Go to to the IMF site
- Figure out the url
- Go there
- Right click on the data
- Save to downloads directory ...
- Run our little translator ...
- ...

Two issues with that. 

- One, it might take you most of the mornig, and it's easy to forget a step — a note that was obvious to you when you wrote it might be pretty cryptic three months later.
- Two is you can't tell a computer to do that!

So let's make that to-do list more computer-friendly with something called a Makefile.

**SCREEN**

- Let's start by firing up your codespace.
- Remember, to return to work, go to github.com/codespaces
- Click on the title "My class codespace"
- Restart codespace
- Give it a moment, and you should be at this screen.
- If you don't see your files here, click the files icon
    - There are a lot of things here ... most you don't need to worry about
    - Do want to highlight two files we'll be playing with
    - `Makefile`
    - `imf_to_csv.js` ... our converter script
- If you don't see a lower horizonal window here, 
    - click Menu > View > Terminal
    - make sure you're on "Terminal" in the bar there.
- Down in the terminal is where we can run commands. Tell the computer to do things.
- Like print something: `echo 'hello world'`
- Or add two numbers: `expr 2 + 2`
- A Makefile is where we can collect a bunch of those commands to run in sequence
- Click on the `Makefile`
- It'll be blank ... but we're going to add to it.

```makefile

greeting:
	echo 'hello'

math:
	expr 2 + 2

then ..

all: greeting math
```

couple of things ... 

- the colons are important
- and where we use TABs, they are imporant, too ... those aren't spaces.
- Pretty much anything you can run from the "command line" ... your terminal ... can be in a Makefile

Let's use one of those: `mkdir`, for make directory

```
directories:
	-mkdir tmp
	-mkdir data
```

- added a "-" at the start of the command ... 
    - basically ignores an error if it happens, like if it already exists
- run `make directories`
- see the directories appear? 

Let's save our work ... 

- click on the Source Control button
- Find the plus sign and click that
- add a phrase, starting with a past-tense verb, like  "added commands to makefile"
- commit
- publish

**OUTRO**

Now that you have a sense of the Makefile, we'll start using it to make our map. That's next time.

### Video 2-3

**INTRO**

Let's continue working with our Makefile so we can automate our data-handling for our inflation map — and how you might do it for other data.

**SCREEN**

- OK, we're still in our codespace working on our Makefile
- Remember how I had to download the inflation and country files? I used my mouse to right-click on the web page and downloaded it to my computer.
- We can let the Makefile do the downloading for us!
- Open your Makefile, if it's not already open
- We're going to use a command-line command called "curl" to download our file, just like the browser did.
- Here's the URL I used for the data ...

```
downloads:
    curl "https://www.imf.org/external/datamapper/api/v1/PCPIPCH?periods=2023" -o tmp/inflation.json
```

- Oh, man. Do I have to type that???
- This is where I share a little secret ... that all of the code we'll be using is already in your codespace, in "prebaked" and "makefile" with a little m.
- So you can copy and paste!
- Don't do it all at once, though. Only use the chunks we need in sequence for now.
- the "-o" means output the file to ...
- Also: curl `"https://www.imf.org/external/datamapper/api/v1/countries" -o tmp/countries.json`
- Terminal: Now make downloads
- Check the files
- OK! let's run our little converter script
- copy over the freshdata block

```
freshdata:
	node imf_to_csv.js

all: directories downloads freshdata

clean:
	-rm -rf ./data
	-rm -rf ./tmp
```

- make all

- you can right-click (control-click on mac) on that file to download it to your computer

Let's save our work ... this takes a few steps

- click on the Source Control button
- click on the pluss sign
- add a phrase, starting with a verb "added commands to makefile"
- commit
- publish

**OUTRO**

So whenever we need to update that map ... maybe next quarter or next month when that data is updated ... we can use "make all" to very easily get the fresh data and put it into the format we need. We don't even need to remember exactly how we did it :-) 

Then we can just file and upload it to our existing Datawrapper map.

Next up, some other things you can do with Makefiles.

### Video 2-4

**INTRO**

Pretty much anything you can do on the command line, you can put into a Makefile. This can even include image manipulation and mapmaking.

One of my favorite command-line tools is mapshaper, made by my new york times colleague Matt Bloch. It's free and open to the public, and it's super powerful for anyone working with geospatial data.

**SCREEN**

Figure out shapefiles I can turn into geojson and svg
use node_modules/.bin/mapshaper

- Example: https://droughtmonitor.unl.edu/CurrentMap.aspx
- Data
- GIS data
- Current ... right-click on shapefile ... copy link address

Download that shapefile

I can go to the US Census and get a map of the united states

- https://www.census.gov/geographies/mapping-files/time-series/geo/cartographic-boundary.html (bit.ly/mapping-census)
- Then I can use mapshaper on the command line (bit.ly/mapshaper-commands)


**OUTRO**

If you were following along, don't forget to shut down your Codespace, by going to github.com/codespaces and clicking "stop codespace"



As you automate your work, finding tools that work from the command line is key.

- Mapshaper for geospatial work
- imagemagick for image manipulation
- ffmpeg for video work

Or using little scripts you find or make yourself, like I did.

Next week, I'll help you get set up using Amazon Web Services.

## Module 3: Storing and hosting your data on Amazon Web Services

Behind the scenes, many journalism maps and interactives are driven by data files publicly available on the internet — and hosted somewhere that can handle a fair amount of traffic. We’ll show you how to use Amazon Web Services for that hosting.

This module will cover:

- An overview of the Amazon Web Services storage service, S3
- Setting up your AWS account
- Making a storage bucket
- Understanding and setting permissions for your bucket
- Uploading files into your bucket

### Video 3-1

NOTES:
https://s3.amazonaws.com/media.johnkeefe.net/class-modules/inflation.csv

CORS  json

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```


**INTRO**

Welcome to Week 3!

Remember that data file I gave you — infation.csv? Think for a moment ... how did I give that file to you? Where does it live.

I have you a shortlink, `bit.ly/knight-data` ... which is a pointer to a file that

Actually lives at `https://s3.amazonaws.com/media.johnkeefe.net/class-modules/inflation.csv`

Note the `amazonaws.com` in the url. The file is actually hosteded on Amazon's AWS service.

It's public, and it's how I shared it with you.

Behind the scenes, many data-visualization projects in journalism are driven by a data file just like this -- sitting on the public web somewhere. Almost all of the maps I showed you in the introduction are using such a data file.

This is because it's a lot easier to alter a data file than it is to rewrite code that's building a map or chart. Instead, the code is designed to read the data file for the latest information, and then put that into the visualization — like our Datawrapper map.

These external files have to have some important characteristics:

- They have to be somehwere public [Pubilc]
- The servers hosting the files have to be capable of handling a lot of traffic [High traffic]
- They have to be using secure protocols, that's the "https" in the url [https]
- We have to give software runing elsewhere permission to use them. (CORS)

Fortunately, this is not a new situation! This is how a lot of the internet operates, and cloud computing companies have ways to handle all of this for us. We're going to use Amazon Web Services or AWS, tho Google, Microsoft and others also provide this service.

I'll show you how to upload files to AWS manually, and more importantly — automatically!

I want to take a moment to talk about the first one on our list: the files have to be somehwere public. We want to make it easy for Datawrapper — or anyone else looking at your map in a web browser — to get this data. So the spot we're going to put that file is PUBLIC. Anyone can and will see it — and ANY OTHER FILES IN THAT LOCATION.

So don't keep private files there, and make sure you data files don't have any private information in them — even in columns or fields you might not be using for your map or chart. Also ... don't assume the file names or locations are too obscure to find; people (or bots) can and will find them. Many of the data breaches you've heard about were because people kept private information in these public locations. Don't do it.

We're mapping public data and don't have any extraneous columns — just country names, country codes and inflation — all public information. So we're good. We don't care if people find the data. In fact, we're FEATURING it on our map, just in a prettier form.

That's next. 


**SCREEN**

**OUTRO**

Next up, I'm going to show you how to use Amazon Web Services to put your data files in a place Datawrapper use it. It's also how many other data visualization projects get the data to display. 



### Video 3-2

**INTRO**

OK, let's get started with hosting our data file in Amazon Web Services. We're going to make a storage bucket you can use for all of your projects ... not just this one. I'm going to move through this process at a healthy pace, but feel free to pause the video as you follow along.

**SCREEN**

- Sign in to your AWS account: https://aws.amazon.com/
    - "Sign in"
- In top search bar "S3" Simple Storage System
- Create bucket
- Pick the other ACL option
- Bucket name: unique to the world! just letters, numbers and hyphens.
    - Something like "jkeefe-projects-pubic"
    - You'll be able to use this bucket for any projects or file sharing
    - Put "-public" at the end to remind yourself that it is, indeed, public
- UNCHECK the "Block all public access" box
- CHECK the acknowledgement
- Create bucket button
- Find bucket name
- Click it
- Permissions ... 
- Scroll down to Cross-origin resource sharing  (CORS)
- Click edit
- Open new tab
- Use a gist to share it, with bitly: `bit.ly/knight-cors`
- Click raw
- Paste in
- Save changes

**OUTRO**

OK ... we've got our bucket set up. Next, we'll test it out.

### Video 3-3

**INTRO**

Now that we have a public bucket where we can share data, let's put some data into it.

**SREEN**

- Sign in to your AWS account: https://aws.amazon.com/
    - "Sign in"
- In top search bar "S3" Simple Storage System
- Click your bucket name
- Click "Objects"
- create a folder called `inflation-map`
- click on that folder

OK!

Upload our inflation file

- click upload button
- Click "Add Files"
- find and pick the file
- click "Upload"
- chose the file once uploaded
- choose: make public using ACL
- explain "Failed to ..." 

Test it in your browser ...

- find the url for the file
- copy url with little copy icon
- make a new tab
- paste it
- should show or just download.

Let's test it on datawrapper!

- Click on the item
- Get the url
- Open new tab with Datawrapper
- Put that file into the box instead
- Green checkmark?
- It's working!

**OUTRO**

I don't want to understate this ... you now have REMOTE CONTROL over the data on that inflation map. If you change the values in that CSV and upload it again into the same bucket, with the same name, the colors on the map will change!

Next we're going to grant permissions to a bot — instead of us — to upload those files for us. 

### Video 3-4

**INTRO**

We just uploaded a file to AWS manually ... but soon, we're going to want a computer to do that uploading for us. That computer is going to need permission to do so. One (bad) way to do that is just hand over your superuser login credentials. But if someone got ahold of those, or you accidentally pasted that pubically, anyone could run your AWS account. Which you don't want.

So we're need to make a robot user, with limited permissions — just write-access to ths one bucket.

**SCREEN**

- Policies
    - Create policy
    - JSON
    - open new tab ... `bit.ly/upload-policy`
    - change to your bucket name
    - save
- User
    - name: github-uploads
    - Attach policies directly
    - May have to use the reload button
    - Check box next to access my bucket
    - Scroll down to "next"
    - "Create user"

That's all for now.

**OUTRO**

You might not realize it, but with the Makefile and hosting the file on Amazon Web Services, we've laid the groundwork to automatically update our map. Next week, we'll put that into action ... with Github Actions.

## Module 4: Automating the entire process using Github actions — Plus: Making a Slack bot!

<!-- Lets tied together everything we've learned so far to automatically upload our data and update our map using Github Actions. Then we’ll use the same techniques to make a bot that can alert us when an online data set changes via Slack, a messaging platform used by many newsrooms. (Even if you don’t use Slack, we’ll show you how to set up your own version.)

This module will cover:

- Building and running a Github Action
- Scheduling a Github Action using cron
- Using Github Actions to automate a map update 
- Sending a slack message when the map has new data
- Monitoring other data sets -->

**NOTES**


### Video 4-1

**INTRO**

At this point, we can make a map by firing up a computer in the cloud and running "make all." That makes a file we can make public on Amazon Web Services for Datawrapper to use in our map. With a service called Github Actions, we can do all of that in one step ... and even schedule the computer to run it without us!

**SCREEN**

Github actions essentially does this:

When prompted, Github Actions

- Spins up a fresh computer
- Loads up all the code in our repository
- Runs any commands we wish
- And shuts down ... poof!

So to tell the computer to do all those things, we have a file in ...

- .github/workflows ... say-hello.yaml
- This is the "Action" or worksflow
- Let's take a look
    - Go through the steps
- So where does this run? In the cloud! We can make it go here:
    - Go online to your github account, pick `knightcenter-data-mapping`
    - Click the Actions tab
    - click say-hello
    - run workflow
    - run workflow again
    - whee!
    - it's running
    - what did it do?
    - click on this run
    - click on build
    - click on "Process data"
 
    -  Take a look at the other workflow file: update-map.yaml
    - Step through it
    - THere's our "make all"
    - Additional part: Upload to S3

**OUTRO**

That's the basics of a Github Action!

Next up, we'll get set up for automatically updating our inflation map.

### Video 4-2

**INTRO**

There's one more administrative task we need to take care of. In order to let Github actions upload data to our S3 bucket, we need to give it the credentials — basically the username and password — to do that. 

**SCREEN**

- Open a new tab and navigate to your repo
- Pick settings > secrets and variables > actions
- New repository secret
    - AWS_S3_BUCKET (all upper case, underscores between them)
- "View User"


New Tab: AWS

- console.aws.amazon.com
- Search S3
- Find your bucket, copy the name

Github tab:

- Paste in the "Secret" box.
- Add Secret
- New repository secret
    - AWS_ACCESS_KEY_ID
     
Back to AWS tab:

- Searh IAM
- Pick IAM
- Users
- github-upload
- "Security credentials" tab
- Scroll down to "access keys"
- Create access key
- "Application running outside AWS"
- Read cautions
- Click "Next"
- "Create Access Key"
- "Download CSV file" ... for safety ... keep in a safe place I put mine in my password manager ... if you want to make a new project, in a new Github repo, you'll need these again.
- Click on the copy icon next to Access Key (not Secret Access Key)

Back to Github Tab

- Paste into "Secret"
- New repository secret
    - AWS_SECRET_ACCESS_KEY

Back to the AWS Tab

- Next to "Secret Access Key" click "Show"
- click the "Copy" icon next to it

Back to Github Tab

- Paste into "Secret"

OK!

Does it work? Let's try:

- Actions
- make-map
- Run workflow!

**OUTRO**

We've made it so we can update our inflation map with a click of a button.

But even better, we can _schedule_ the Github action to run on it's own

Now, if you didn't get a green checkmark and want to troubleshoot your issue, you have some options:

- Search the XX forum to see if others have had it
- Post there if not
- Join me for one of our live event next week


### Video 4-3

**INTRO**

Something kind of great, is that we can schedule retular tasks ... like update a map once a month. In the computer world, when something needs to run on a schedule, often the operation doing that scheduing is called "cron" or "crontab" or a "cron job"

**SCREEN**

- to check this out, go to `crontab.guru`
- experiment
- end up on `15 12 1 * *` 
- UTC!
- Drought map is posted evey Thursday morning, so you could automatically update that

- Github action: 
    - Every first day of the month at 12:15 p.m. UTC
    - We copy our files into a cloud computer
    - Run "make all"
    - which does ...
    - Then we upload it all to S3

- Datawrapper checks that file whenever we load a map
    - So we have fresh data!
- We could also run it manually
- And we can see what is happening

**OUTRO**

So how could you use this?

Basically, anything you can run on the command line you can use in a Makefile. There are lots of examples of how to do that if you need.

And this bucket we made is something you can keep using!

Next up ... using a github action to watch for changes in data online.

### Video 4-4

NOTES: https://api.slack.com/messaging/webhooks

Cool tweak ...

- Follow the instructions for making a Slack webhook: `https://api.slack.com/messaging/webhooks`
- Add that to the Github Secrets as (SLACK_WEBHOOK)
- Add "filecheck" to the makefile
- Add the "filecheck" to the make all command
