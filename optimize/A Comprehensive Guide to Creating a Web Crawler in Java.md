
# A Comprehensive Guide to Creating a Web Crawler in Java

**Web crawlers** are essential tools used to navigate the web and discover new or updated pages for indexing. A web crawler typically starts with a collection of seed websites or popular URLs and uses breadth and depth-first searches to extract hyperlinks.

A web crawler must be **kind** and **robust**. Kindness implies respecting rules set in a website's `robots.txt` file and avoiding frequent visits. Robustness means avoiding traps like spider webs and other malicious behaviors.

---

## Steps to Create a Web Crawler

To build a basic web crawler, follow these steps:

1. Pick a URL from the queue (frontier).
2. Fetch the HTML code for the URL.
3. Parse the HTML code to extract links to other URLs.
4. Check if the URL or its content has already been crawled. If not, add it to the index.
5. For each extracted URL, verify whether it permits crawling (`robots.txt`, crawling frequency, etc.).

---

### Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Using Jsoup for HTML Parsing in Java

We use **Jsoup**, a popular Java HTML parsing library, to fetch and parse web pages. Add the following dependency to your `POM.xml` file to use Jsoup:

```xml
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.10.2</version>
</dependency>
```

---

## Basic Web Crawler Code Example

Here is an example of a simple Java-based web crawler:

### `WebCrawlerExample.java`

```java
// Import necessary packages
package javaTpoint.javacodes;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import java.io.IOException;
import java.util.HashSet;

public class WebCrawlerExample {
    private HashSet<String> urlLink;

    public WebCrawlerExample() {
        urlLink = new HashSet<>();
    }

    public void getPageLinks(String URL) {
        if (!urlLink.contains(URL)) {
            try {
                if (urlLink.add(URL)) {
                    System.out.println(URL);
                }

                Document doc = Jsoup.connect(URL).get();
                Elements linksOnPage = doc.select("a[href]");

                for (Element link : linksOnPage) {
                    getPageLinks(link.attr("abs:href"));
                }
            } catch (IOException e) {
                System.err.println("For '" + URL + "': " + e.getMessage());
            }
        }
    }

    public static void main(String[] args) {
        WebCrawlerExample obj = new WebCrawlerExample();
        obj.getPageLinks("https://www.javatpoint.com/digital-electronics");
    }
}
```

The above program extracts all hyperlinks from a webpage and prints them to the console.

---

### Modifying the Code for Depth-Based Crawling

To limit the depth of crawling, modify the crawler to include a depth counter. This prevents the crawler from going beyond a specified level.

### `WebCrawlerExampleWithDepth.java`

```java
// Import necessary packages
package javaTpoint.javacodes;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import java.io.IOException;
import java.util.HashSet;

public class WebCrawlerExampleWithDepth {
    private static final int MAX_DEPTH = 2;
    private HashSet<String> urlLinks;

    public WebCrawlerExampleWithDepth() {
        urlLinks = new HashSet<>();
    }

    public void getPageLinks(String URL, int depth) {
        if (!urlLinks.contains(URL) && depth < MAX_DEPTH) {
            System.out.println(">> Depth: " + depth + " [" + URL + "]");
            try {
                urlLinks.add(URL);
                Document doc = Jsoup.connect(URL).get();
                Elements linksOnPage = doc.select("a[href]");
                depth++;

                for (Element link : linksOnPage) {
                    getPageLinks(link.attr("abs:href"), depth);
                }
            } catch (IOException e) {
                System.err.println("For '" + URL + "': " + e.getMessage());
            }
        }
    }

    public static void main(String[] args) {
        WebCrawlerExampleWithDepth obj = new WebCrawlerExampleWithDepth();
        obj.getPageLinks("https://www.javatpoint.com/digital-electronics", 0);
    }
}
```

---

## Key Differences: Data Crawling vs. Data Scraping

- **Data Crawling**: Involves navigating and indexing large datasets. A crawler visits multiple web pages and indexes all the accessible information.
- **Data Scraping**: Involves retrieving specific information from a webpage or dataset.

---

## Crawling and Extracting Articles in Java

The following example demonstrates how to extract article titles and links using Jsoup:

### `ExtractArticlesExample.java`

```java
package javaTpoint.javacodes;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

public class ExtractArticlesExample {
    private static final int MAX_DEPTH = 2;
    private HashSet<String> urlLinks;
    private List<List<String>> articles;

    public ExtractArticlesExample() {
        urlLinks = new HashSet<>();
        articles = new ArrayList<>();
    }

    public void getPageLinks(String URL, int depth) {
        if (urlLinks.size() != 50 && !urlLinks.contains(URL) && depth < MAX_DEPTH &&
            (URL.startsWith("http://www.javatpoint.com") || URL.startsWith("https://www.javatpoint.com"))) {
            System.out.println(">> Depth: " + depth + " [" + URL + "]");
            try {
                urlLinks.add(URL);
                Document doc = Jsoup.connect(URL).get();
                Elements linksOnPage = doc.select("a[href]");
                depth++;

                for (Element link : linksOnPage) {
                    if (link.attr("abs:href").startsWith("http://www.javatpoint.com") || 
                        link.attr("abs:href").startsWith("https://www.javatpoint.com")) {
                        getPageLinks(link.attr("abs:href"), depth);
                    }
                }
            } catch (IOException e) {
                System.err.println("For '" + URL + "': " + e.getMessage());
            }
        }
    }

    public void getArticles() {
        for (String URL : urlLinks) {
            try {
                Document doc = Jsoup.connect(URL).get();
                Elements articleLinks = doc.select("a[href]");

                for (Element link : articleLinks) {
                    if (link.text().contains("java")) {
                        ArrayList<String> temp = new ArrayList<>();
                        temp.add(link.text());
                        temp.add(link.attr("abs:href"));
                        articles.add(temp);
                    }
                }
            } catch (IOException e) {
                System.err.println(e.getMessage());
            }
        }
    }

    public void writeToFile(String fileName) {
        try (FileWriter writer = new FileWriter(fileName)) {
            for (List<String> article : articles) {
                String data = "- Title: " + article.get(0) + " (link: " + article.get(1) + ")\n";
                writer.write(data);
            }
        } catch (IOException e) {
            System.err.println(e.getMessage());
        }
    }

    public static void main(String[] args) {
        ExtractArticlesExample obj = new ExtractArticlesExample();
        obj.getPageLinks("https://www.javatpoint.com", 0);
        obj.getArticles();
        obj.writeToFile("Articles.txt");
    }
}
```

---

## Conclusion

Web crawling and scraping are fundamental tools for data gathering and analysis. Using Java and Jsoup, you can build robust crawlers capable of extracting data at various depths and configurations. Start with simple implementations and expand as needed for more complex tasks.

### Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)
