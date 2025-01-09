
# Google News Scraping in R: A Step-by-Step Guide

## Introduction

[Google News](https://en.wikipedia.org/wiki/Google_News) is a popular news aggregator that allows users to search for news from diverse sources. While many are familiar with Google News alerts, it can also be used to scrape news stories for text analysis purposes. This guide demonstrates how to use the **`tidyRSS`** and **`rvest`** R packages to:

1. Generate a URL for a Google News RSS feed.
2. Read the RSS feed using `tidyRSS`.
3. Use `rvest` to scrape the article content from the provided links.

### Important Disclaimer
Web scraping may violate a website's terms of service or copyright laws. Be cautious when implementing these techniques and avoid overloading the servers. Additionally, frequent scraping attempts may result in your IP address being blocked.

---

### Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Step 1: Generate a Google News RSS Feed URL

Google News offers a convenient feature that allows users to create RSS feeds based on specific search terms. RSS feeds summarize information (e.g., news articles) in a machine-readable format, making them perfect for automation.

For instance, to retrieve BBC articles about Boris Johnson's comments on Ukraine, Putin, and Russia, you could use the following search term:

**Search term**:  
`"Boris Johnson" AND ("Ukraine" OR "Putin" OR "Russia") site:bbc.co.uk`

This query retrieves all relevant BBC coverage. To generate a Google News RSS feed URL, use the following R code:

```r
search_term <- '"Boris Johnson" AND ("Ukraine" OR "Putin" OR "Russia") site:bbc.co.uk'
url <- paste0('https://news.google.com/rss/search?q=', URLencode(search_term, reserved = TRUE), '&hl=en-MY&gl=MY&ceid=MY:en')
```

Here:
- **`URLencode`** converts the search term into URL-friendly text.
- The **`&hl=en-MY&gl=MY&ceid=MY:en`** portion specifies language and location preferences (English for Malaysia). Customize these parameters for reproducibility.

---

## Step 2: Use `tidyRSS` to Read the RSS Feed

The `tidyRSS` package allows you to import RSS feed data into R as a structured dataframe. Google News RSS feeds provide up to 100 results. Here's how to retrieve the data:

```r
library(tidyRSS)
articles <- tidyfeed(url)
```

The output, `articles`, is a dataframe containing:
- `item_title`: The article's title.
- `item_description`: A brief summary.
- `item_link`: A link to the original article.

---

## Step 3: Scrape Article Content with `rvest`

Once you have the links, you can use the `rvest` package to scrape full article content. Web scraping automates the extraction of data from web pages. The following code loops through each article link and scrapes the paragraph content:

```r
library(rvest)
all_para <- c()

for (n in 1:nrow(articles)) {
  html <- read_html(articles$item_link[n])
  para <- html_text(html_nodes(html, 'p'))
  all_para <- c(all_para, para)
  Sys.sleep(10) # Pause for 10 seconds between requests
}
```

**Key Details**:
- **`read_html`** reads the HTML content of a webpage.
- **`html_nodes`** selects the paragraph tags (`<p>`).
- **`Sys.sleep(10)`** introduces a 10-second delay to prevent IP blocks, especially when scraping the same domain repeatedly.

### Filtering Duplicate and Irrelevant Text
Not all scraped text is relevant—some may include ads or copyright notices. To filter duplicates, use the following code:

```r
para_tbl <- as.data.frame(table(all_para))
para_tbl <- subset(para_tbl, Freq < 2)
```

This creates a cleaned dataframe, `para_tbl`, containing only unique paragraphs.

---

## Step 4: Save the Scraped Data

After cleaning, save the scraped content for future use. You can save the data as a CSV file using this code:

```r
write.csv(para_tbl, 'mycorpus.csv', row.names = FALSE)
```

A manual review of the data is recommended for smaller datasets to ensure accuracy and remove any irrelevant content.

---

## Conclusion

Scraping Google News using R provides a systematic way to gather news content for text analysis. By combining `tidyRSS` and `rvest`, you can create a custom dataset tailored to your research or business needs.

Remember to respect website terms of service and use scraping tools responsibly. Happy scraping!

---

### Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)
