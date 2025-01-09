
# Accelerating Web Scraping with Asynchronous Python

Web scraping is a popular and powerful way to gather data from the internet, whether it’s fetching details from APIs or collecting data from numerous web pages. In this guide, we’ll explore how to make web scraping faster and more efficient by leveraging **asynchronous programming**.

## What Does Asynchronous Mean?

Most programs run **synchronously**. For example, in a simple for-loop:

```python
for fruit in ["apple", "orange", "banana"]:
    print(fruit)
```

The output happens sequentially—"apple" is printed first, then "orange," and finally "banana." Similarly, synchronous web requests process one request at a time.

### Synchronous Requests Example

Let’s consider an example where we use the `requests` library to fetch web pages, `BeautifulSoup` to parse them, and `Pandas` to store the data in a dataframe. We aim to scrape stories, their scores, and the sites from the Hacker News front page.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Example of synchronous web scraping
```

Here’s the result of fetching and processing the first five pages:

```plaintext
Time taken: 4.73 seconds
                                               Title  Score               Site
0  Sweet Home 3D is a free interior design applic...     54    sweethome3d.com
1                                    Please Buy Less     74  pleasebuyless.com
2  RoughViz: JS library for creating sketchy/hand...    278         github.com
3      Off-Grid Cyberdeck: Raspberry Pi Recovery Kit    488           back7.co
4  TikTok: An update on recent content and accoun...      7         tiktok.com
5      Lessons learned building an ML trading system    475   tradientblog.com
6  Go master Lee Se-dol says he quits, unable to ...    468          yna.co.kr
7  EHTML: Front-end development can and should be...     38         guseyn.com
8                    Twitter CEO Endorses DuckDuckGo     72        twitter.com
9  Show HN: I made an open-source anonymous email...    256       anonaddy.com
```

Fetching five pages took about **five seconds**, but increasing the number of pages to 25 led to a proportional increase in time:

```plaintext
Time taken: 24.34 seconds
```

### Where Does the Time Go?

Using `time` to measure performance shows:

```plaintext
$ time python3 sync_req.py
real    0m24.900s
user    0m3.634s
sys     0m0.672s
```

- **real**: Total time from start to finish.
- **user**: Time spent executing the process on the CPU.
- **sys**: Time spent on system-level tasks (e.g., memory allocation).

In this case, most of the time was spent waiting for I/O rather than processing data. How can we reduce this inefficiency?

## Faster Web Scraping with Asynchronous Requests

When the order of requests doesn't matter, as in most web scraping tasks, you can process requests **asynchronously**, reducing wait times and improving performance.

### Implementing Asynchronous Web Scraping

Using `ThreadPoolExecutor`, we can execute multiple threads simultaneously. This allows our CPU to handle multiple requests in parallel. Let’s update our code:

```python
from concurrent.futures import ThreadPoolExecutor
# Example of asynchronous scraping
```

By distributing the workload across multiple threads, we can scrape 25 pages much faster. Here’s the time comparison:

#### Synchronous vs. Asynchronous Performance

| **Method**       | **Pages Scraped** | **Time Taken**  |
|-------------------|-------------------|-----------------|
| Synchronous       | 25               | 24.34 seconds   |
| Asynchronous (4 threads) | 25        | 9.34 seconds    |
| Asynchronous (12 threads) | 25       | 3.55 seconds    |

### Key Takeaways

Using asynchronous methods, our 12-thread scraper completed the same task in just **3 seconds**, compared to **24 seconds** with a single-threaded approach. This demonstrates the significant efficiency boost asynchronous programming can bring to web scraping.

## Boost Your Web Scraping Efficiency

Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

Asynchronous techniques are not limited to scraping—they can be applied to any I/O-intensive task. Leveraging these methods can save you time and resources while achieving better performance.

---

*Written on November 27, 2019 by Philipp.*
