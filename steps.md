
# Workshop at the Buenos Aires Media Party, October 2023

## Setup: Github Codespaces

We'll get the code and get started with Github Codespaces

### References

- https://docs.github.com/en/codespaces/getting-started/quickstart
- Pricing: https://docs.github.com/en/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces

### Steps

![Actions Workshop - Media Party - Oct 2023 001](https://github.com/jkeefe/actions-workshop/assets/312347/dd4d7d83-c420-432e-87a4-b04003b6b950)

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

### Drought Monitor

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

### Slack monitor

NOTES: https://api.slack.com/messaging/webhooks

Cool tweak ...

- Follow the instructions for making a Slack webhook: `https://api.slack.com/messaging/webhooks`
- Add that to the Github Secrets as (SLACK_WEBHOOK)
- Add "filecheck" to the makefile
- Add the "filecheck" to the make all command


