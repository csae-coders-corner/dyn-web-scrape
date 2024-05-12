# Dynamic web scraping using Rselenium.

A helpful post in an earlier coder's corner post dealt with webscraping using rvest. Rvest allows you to scrape text and download files in static webpages, but RSelenium completes the webscraping toolkit, allowing you to actively interact with the webpages themselves. Let's say we need to input log-in details, select drop-down menus and even click next-page buttons - the RSelenium package is powerful enough for all of the above.

In this post, I showcase a small case study of using R to interact with a webpage, specifically the European Competition Authority's library of rulings. This task would be the challenge before scraping the website to download all relevant rulings of interest.

## Part I – Installing Docker



RSelenium works by operating a version of an internet browser that R can directly interact with. One of the easiest ways to create this virtual browser is through installing Docker[link]. Docker creates a virtual browser for Rselenium, and by extension us, to directly send commands to the browser. It also allows for our code to be reproducible and run bug-free, as we can specify exactly which browser version and type our code should run with.

After installing docker, using the command prompt to begin running the browser - 

```
docker run -d -p 4445:4444 selenium/standalone-firefox:2.53.0
```
For further help, refer directly to https://docs.ropensci.org/RSelenium/articles/docker.html.

## Part II – Setting up Rselenium

Having made a version of the browser within Docker, and after installing RSelenium, we connect RSelenium with the existing browser.


```
  
  # initialzing virtual environment

fprof <- makeFirefoxProfile(list(
  "pdfjs.disabled"=TRUE,
  "plugin.scan.plid.all"=FALSE,
  "plugin.scan.Acrobat" = "99.0",
  "browser.helperApps.neverAsk.saveToDisk"='application/pdf'))
# Open a firefox browser
remDr <- remoteDriver(remoteServerAddr = "localhost", port = 4445L, extraCapabilities = fprof )
remDr$open()


```
makefirefoxProfile lets us set some default behaviour of our virtual firefox browser. Also note that the Ports and browser correspond to the browser we created with Docker. 

To check if this worked, open a webpage in Rselenium using 

```
remDr$navigate("https://ec.europa.eu/competition/elojade/isef/index.cfm?clear=1&policy_area_id=1")

```

To see what R is seeing, let's take a screenshot and ask R to display the image

```
remDr$screenshot(display = TRUE)

```
![image](https://github.com/csae-coders-corner/dyn-web-scrape/assets/64132992/050f86db-320f-4bb1-8119-7ba0c9ac16b3)



Now that we know that Rselenium can access the internet, we can move to interacting directly with the website using the retinue of commands that RSelenium puts at our disposal.




## Part II – Interacting with the webpage


Our goal is to set the search menu in the webpage to filter for all cartel cases in the database,  from 8-9-2007 to 8-9-2021. Additionaly we also want to input sector codes  and select specific document types.





```






