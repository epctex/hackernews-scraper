[https://apify.com/epctex/hackernews-scraper](https://apify.com/epctex/hackernews-scraper?fpr=yhdrb)

# Actor - Hacker News Scraper

## Hacker News scraper

Since Hacker News doesn't provide a good API, this actor should help you to retrieve data from it.

The Hacker News data scraper supports the following features:

-   Scrape front page listing - You can scrape the homepage listings with any page you want.

-   Scrape the newest listing - Latest news can be scraped right away from Hacker News.

-   Scrape historical data - If you are looking for historical data, you can pick any date you want and scrape it over.

-   Scrape listings of Ask HN - If you are specifically looking for an "Ask HN" type of listing, you can target it.

-   Scrape listings of Show HN - If you are specifically looking for a "Show HN" type of listing, you can target it.

-   Scrape listing details - You can scrape a single listing.

-   Scrape Hacker News Algolia - You can retrieve everything from Hacker News' Algolia website

-   Scrape job listings - You can scrape the latest job listings that are posted on Hacker News.

## Bugs, fixes, updates, and changelog

This scraper is under active development. If you have any feature requests you can create an issue from [here](https://github.com/epctex/hackernews-scraper/issues).

## Input Parameters

The input of this scraper should be JSON containing the list of pages on Hacker News that should be visited. Possible fields are:

- `startUrls`: (Optional) (Array) List of Hacker News URLs. You should only provide a news list, jobs list, or detail URLs.

- `mode`: (Optional) (String) Mode of the actor. Can be `FRONTPAGE`, `NEWEST`, `ASK`, `SHOW`, `JOBS` or `PAST`.

- `enableCommentHierarchy`: (Optional) (Boolean) Enables comment hierarchy over the posts. It retrieves all the comments and builds a tree within the replies.

- `endPage`: (Optional) (Number) Final number of page that you want to scrape. The default is `Infinite`. This applies to all `search` requests and `startUrls` individually.

- `maxItems`: (Optional) (Number) You can limit scraped items. This should be useful when you search through the big lists or search results.

- `proxy`: (Required) (Proxy Object) Proxy configuration.

- `extendOutputFunction`: (Optional) (String) Function that takes a JQuery handle ($) as an argument and returns an object with data.

This solution requires the use of **Proxy servers**, either your own proxy servers or you can use [Apify Proxy](https://www.apify.com/docs/proxy).

### Tip

When you want to scrape over a specific listing URL, just copy and paste the link as one of the **startUrl**.

If you would like to scrape only the first page of a list then put the link for the page and have the `endPage` as 1.

With the last approach that is explained above you can also fetch any interval of pages. If you provide the 5th page of a list and define the `endPage` parameter as 6 then you'll have the 5th and 6th pages only.

If you would like to scrape historical data (ex: 2020-03-18) go to Hacker News, click on the "Past" tab, and find the URL that you are looking for. Then use the link as **startUrl**. Also; this is the format of historical data: `https://news.ycombinator.com/front?day=2020-03-18`

### Compute Unit Consumption

The actor is optimized to run blazing fast and scrape many listings as possible. Therefore, it forefronts all listing detail requests. If the actor doesn't block very often it'll scrape 100 listings in 1 minute with ~0.03-0.04 compute units.

### Hacker News Scraper Input example

```json
{
    "startUrls": [
        {
            "url": "https://news.ycombinator.com/item?id=26501527"
        },
        {
            "url": "https://news.ycombinator.com/front?day=2020-03-18"
        }
    ],
    "mode": "FRONTPAGE",
    "enableCommentHierarchy": false,
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

If you provide incorrect input to the actor, it will immediately stop with a failure state and output an explanation of what is wrong.

## Hacker News Export

During the run, the actor stores results into a dataset. Each item is a separate item in the dataset.

You can manage the results in any language (Python, PHP, Node JS/NPM). See the FAQ or <a href="https://www.apify.com/docs/api" target="blank">our API reference</a> to learn more about getting results from this Hacker News actor.

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

### Single Comment

```json
{
    "scrapedType": "comment",
    "userName": "tsl54",
    "userLink": "https://news.ycombinator.com/user?id=tsl54",
    "age": "9 months ago",
    "message": "Congratulations on getting here! I’ve been part of a few management changes from the board level.",
    "comments":[]
},
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

## Contact
Please visit us through [epctex.com](https://epctex.com) to see all the products that are available for you. If you are looking for any custom integration or so, please reach out to us through the chat box in [epctex.com](https://epctex.com). In need of support? [devops@epctex.com](mailto:devops@epctex.com) is at your service.
