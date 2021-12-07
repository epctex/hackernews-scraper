# Actor - Hacker News Scraper

## Hacker News scraper

Since Hacker News doesn't provide a good API, this actor should help you to retrieve data from it.

The Hacker News data scraper supports the following features:

-   Scrape front page listing - You can scrape the homepage listings with any page you want.

-   Scrape newest listing - Latest news can be scraped right away from Hacker News.

-   Scrape historical data - If you are looking for historical data, you can pick any date you want and scrape it over.

-   Scrape listings of Ask HN - If you are specifically looking for "Ask HN" type of listing, you can target it.

-   Scrape listings of Show HN - If you are specifically looking for "Show HN" type of listing, you can target it.

-   Scrape listing details - You can scrape a single listing.

-   Scrape job listings - You can scrape latest job listings that posted on Hacker News.

## Bugs, fixes, updates and changelog

This scraper is under active development. If you have any feature requests you can create an issue from [here](https://github.com/tugkan/hackernews-scraper/issues).

### Incoming Changes

-   Implement nesting on comment replies.

## Setup & Usage

You can see how this actor works these videos:

### Using Start URLs

Watch how to set up Start URLs for Hackernews Scraper [here](https://www.youtube.com/watch?v=PZ17RQiykNE).

[![Apify - Hackernews Scraper - Start URLs](https://i.imgur.com/PWc4eXZ.png)](https://www.youtube.com/watch?v=PZ17RQiykNE)

You can see the output of this example run [here](https://api.apify.com/v2/datasets/0zLBMJHf1a2lXKBGT/items?clean=true&format=json).

### Using Filters

Watch how to set up Mode for Hackernews Scraper [here](https://www.youtube.com/watch?v=PYColthcxa8).

[![Apify - Hackernews Scraper - Using Mode](https://i.imgur.com/49pD2E6.png)](https://www.youtube.com/watch?v=PYColthcxa8)

You can see the output of this example run [here](https://api.apify.com/v2/datasets/lqVZdwz94nKkXB6xs/items?clean=true&format=json).

## Input Parameters

The input of this scraper should be JSON containing the list of pages on Hacker News that should be visited. Required fields are:

| Field                | Type    | Description                                                                                                                  |
| -------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------- |
| startUrls            | Array   | (optional) List of Hacker News URLs. You should only provide news list, jobs list or detail URLs                             |
| maxItems             | Integer | (optional) You can limit scraped products. This should be useful when you search through the big subcategories.              |
| endPage              | Integer | (optional) Final number of page that you want to scrape. Default is `Infinite`. This is applies to all request individually. |
| mode                 | String  | (optional) Mode of the actor. Can be `FRONTPAGE`, `NEWEST`, `ASK`, `SHOW`, `JOBS` or `PAST`.                                 |
| extendOutputFunction | String  | (optional) Function that takes a JQuery handle ($) as argument and returns object with data                                  |
| proxy                | Object  | Proxy configuration                                                                                                          |

This solution requires the use of **Proxy servers**, either your own proxy servers or you can use [Apify Proxy](https://www.apify.com/docs/proxy).

##### Tip

When you want to have a scrape over a specific listing URL, just copy and paste the link as one of the **startUrl**.

If you would like to scrape only the first page of a list then put the link for the page and have the `endPage` as 1.

With the last approach that explained above you can also fetch any interval of pages. If you provide the 5th page of a list and define the `endPage` parameter as 6 then you'll have the 5th and 6th pages only.

If you would like to scrape historical data (ex: 2020-03-18) go to Hacker News, click on "Past" tab and find the URL that you are looking for. Then use the link as **startUrl**. Also; this is the format of historical data: `https://news.ycombinator.com/front?day=2020-03-18`

### Compute Unit Consumption

The actor optimized to run blazing fast and scrape many as listings as possible. Therefore, it forefronts all listing detail requests. If actor doesn't block very often it'll scrape 100 listings in 1 minutes with ~0.03-0.04 compute units.

### Hacker News Scraper Input example

```json
{
    "startUrls": [
        "https://news.ycombinator.com/item?id=26501527",
        "https://news.ycombinator.com/front?day=2020-03-18"
    ],
    "mode": "FRONTPAGE",
    "proxy": {
        "useApifyProxy": true
    },
    "endPage": 1,
    "maxItems": 100
}
```

## During the Run

During the run, the actor will output messages letting you know what is going on. Each message always contains a short label specifying which page from the provided list is currently specified.
When items are loaded from the page, you should see a message about this event with a loaded item count and total item count for each page.

If you provide incorrect input to the actor, it will immediately stop with failure state and output an explanation of what is wrong.

## Hacker News Export

During the run, the actor stores results into a dataset. Each item is a separate item in the dataset.

You can manage the results in any languague (Python, PHP, Node JS/NPM). See the FAQ or <a href="https://www.apify.com/docs/api" target="blank">our API reference</a> to learn more about getting results from this Hacker News actor.

## Scraped Hacker News Properties

The structure of each item in Hacker News listings looks like this:

### Job Listings

```json
{
    "id": "26437893",
    "title": "Substack (YC W18) is hiring to build a better business model for writing",
    "link": "https://substack.com/jobs",
    "age": "7 days ago",
    "scrapedAt": "2021-03-19T21:56:00.085Z"
}
```

### News Listings

```json
{
    "id": "26501262",
    "title": "Fancy Defines",
    "link": "https://idiomdrottning.org/fancy-defines",
    "points": 15,
    "postedUserName": "todsacerdoti",
    "postedUserLink": "https://news.ycombinator.com/user?id=todsacerdoti",
    "numberOfComments": 1,
    "comments": [
        {
            "userName": "nerdponx",
            "userLink": "https://news.ycombinator.com/user?id=nerdponx",
            "age": "2 hours ago",
            "message": "Interesting, I didn't know about this at all. Is it that common in Scheme to write functions that immediately return other functions? Seems like an oddly \"blessed\" usage of syntax that IMO could be better used for something like pattern matching.Looking at this example from the linked SRFI [0]:    (define ((greet-with-prefix prefix) suffix)\n      (string-append prefix \" \" suffix))\n\n    (define greet (greet-with-prefix \"Hello\"))\n\n    (greet \"there!\") => \"Hello there!\"\n\nI'm not convinced that this is anything but an obfuscation, compared to the standard R5RS version:    (define (greet-with-prefix suffix)\n      (lambda (prefix)\n        (string-append prefix \" \" suffix)))\n\n    (define greet (greet-with-prefix \"Hello\"))\n\n    (greet \"there!\") => \"Hello there!\"\n\nWhat do the experienced Schemers think?[0]: https://srfi.schemers.org/srfi-219/srfi-219.html"
        }
    ],
    "age": "4 hours ago",
    "scrapedAt": "2021-03-19T21:37:45.462Z"
}
```
