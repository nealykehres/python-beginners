# More from Mitchell: Web scraping beyond the basics

After chapter 3, Mitchell covers a wide variety of scraping methods and situations. Every website or document you scrape will be different. Your tasks each time are to figure out how to scrape the particular target and how to get the data you want from it.

## Reading and writing files as CSVs

To come.

## Account for hard-to-scrape sites with Selenium and HTTP headers

Sometimes you have to do more so that a website will allow you to scrape it. These two techniques are often necessary, together or separately.

### Selenium, to automate the browser

Mitchell discusses Selenium in **chapter 10**. We can use Selenium together with BeautifulSoup when BeautifulSoup alone is unable to get the contents we want from a web page. Two situations where this comes up: (1) JavaScript is writing the contents into the page after it opens; and (2) contents are not available until you click a button, fill a form, open a menu, etc.

The Selenium documentation is not easy to use. **Use this:** [Getting started with Selenium](http://bit.ly/selenium-intro). You will need to install [Selenium](https://www.seleniumhq.org/) and also a driver for the web browser you want to use (Chrome is good).

When you examine the test scripts (linked to gists in the “Getting Started” doc and also found in this repo), notice that after doing the `driver` stuff, this line creates an `html` variable just like we have been doing all along with BeautifulSoup:

```python
html = driver.page_source
```

Once you have that, you can proceed as usual with a `bsObj` and BeautifulSoup scraping. You do not need to use `driver` and “an entirely new set of selectors” as Mitchell does.

Mitchell uses [PhantomJS](https://github.com/ariya/phantomjs) instead of a physical browser. Note that “it does require a download and installation to use and cannot be installed with pip” (p. 152).

### Sending HTTP headers in your script

Mitchell discusses headers in **chapter 12**. You'll install [Requests](http://docs.python-requests.org/en/master/), a Python library, and you'll discover your own user agent at [WhatIsMyBrowser.com](https://www.whatismybrowser.com/). Find your web browser’s user agent at the bottom of that page.

Mitchell's sample script for sending header information is [here](https://github.com/REMitchell/python-scraping/blob/master/chapter12/1-headers.py). She uses it to go to WhatIsMyBrowser.com, but that's just an example.

I have found I needed to use headers in a scraping script that downloaded messages from a large online forum. The site completely shut out my script until I added a full set of header data:

```python
hdr = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.110 Safari/537.36',
       'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
       'Accept-Charset': 'ISO-8859-1,utf-8;q=0.7,*;q=0.3',
       'Accept-Encoding': 'none',
       'Accept-Language': 'en-US,en;q=0.8',
       'Connection': 'keep-alive'}
```

Then I used the variable `hdr` to create my `bsObj`:

```python
req = session.get(url, headers=hdr)
bsObj = BeautifulSoup(req.text, "html5lib")
```

Notice that I replaced our usual `"html.parser"` with `"html5lib"`. Mitchell used `"lxml"` instead. More about **parsers** below.