
# Google Shopping Scraper: Extract Product Data with Ease

## Introduction

The **Google Shopping Scraper** is a powerful tool designed to extract data from [Google Shopping](https://www.google.com/shopping) across different country domains. It allows you to scrape product details and seller information directly from the first result page, offering detailed insights about each product. Built on the [Apify SDK](https://sdk.apify.com/), this scraper can be executed on the [Apify platform](https://my.apify.com) or locally.

---

## Simplify Your Scraping with ScraperAPI

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Input Parameters

To use the Google Shopping Scraper effectively, you need to provide the following inputs:

| **Field**           | **Type**          | **Description**                                                                                                                                                                   |
|---------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `queries`           | Array of Strings | *(required)* List of search queries. Example: `["iphone 11 pro"]`.                                                                                                              |
| `countryCode`       | String           | *(required)* Specify the country for the search. Use country codes like `US`.                                                                                                   |
| `maxPostCount`      | Integer          | *(optional)* Maximum results to scrape per page. Set `0` for no limit. Note: This scraper only extracts data from the first page (up to 20 results).                            |
| `isAdvancedResults` | Boolean          | *(optional)* Check this to scrape additional data, including `merchantName` and `reviews`.                                                                                      |
| `extendOutputFunction` | String        | *(optional)* Provide a function that uses a JQuery handle (`$`) to customize the output. See the example below for more details.                                                |

### Input Example:
```json
{
  "queries": [
    "iphone 11 pro"
  ],
  "countryCode": "US",
  "maxPostCount": 1,
  "isAdvancedResults": false
}
```

---

## Output

The scraper stores the data in a dataset. Below is an example of an output item:

```json
{
  "shoppingId": "7427975830799030963",
  "productName": "24k Gold Plated Apple iPhone 11 Pro Max - 256 GB Silver Unlocked CDMA",
  "description": "Shoot amazing videos and photos with the Ultra Wide, Wide, and Telephoto cameras. Capture your best low-light photos with Night mode. Watch HDR movies and shows on the 6.5-inch Super Retina XDR display, the brightest iPhone display yet. Experience unprecedented performance with A13 Bionic for gaming, augmented reality (AR), and photography. And get all-day battery life and a new level of water resistance. All in the first iPhone powerful enough to be called Pro.",
  "merchantMetrics": "",
  "seller": [
    {
      "productLink": "http://www.google.com/aclk?sa=L&ai=DChcSEwiT8NStooPoAhUJjrMKHf6KDlkYABABGgJxbg&sig=AOD64_2y1iAG2xTUTL-jllVQRjqJyIg9rw&adurl=&ctype=5",
      "merchant": "eBay",
      "merchantMetrics": "0",
      "details": "· Free shipping",
      "price": "$1,650.00",
      "totalPrice": "$1,796.44",
      "additionalPrice": ""
    }
  ],
  "price": "$1,979.10",
  "merchantLink": "http://www.google.com/aclk?sa=l&ai=DChcSEwjp_vSpooPoAhUMlLMKHR6ODhsYABBGGgJxbg&sig=AOD64_3BSHnJWpFXjeoJyysFuEev97t7Ew&ctype=5&q=&ved=0ahUKEwjFo_GpooPoAhV0mHIEHfKkDGAQg-UECOIG&adurl="
}
```

> **Note**: Price formats may vary based on the country. The scraper retains the format as it appears on the Google Shopping page.

---

## Key Features

### Google SERP Proxy Integration
This scraper leverages the **Google SERP Proxy** to extract localized search results efficiently. For more details, refer to the [Google SERP Proxy documentation](https://docs.apify.com/proxy/google-serp-proxy).

### Extend Output Function
To customize the data output, you can use the `extendOutputFunction`. This function takes a JQuery handle (`$`) as an argument, allowing you to extract additional data fields or modify the existing ones.

#### Return Value:
The return value of this function must be an **object**. Here's what you can do:
- **Add new fields**: Include fields not present in the default output.
- **Modify fields**: Change values of existing fields.
- **Remove fields**: Return existing fields with a value of `undefined`.

#### Example of Adding a New Field:
```javascript
($) => {
  return {
    newField: $("selector").text()
  };
}
```

---

## Expected Costs

The compute units for this scraper are approximately **0.0272 per 10 products**.

For more efficient scraping with fewer obstacles, consider combining this scraper with ScraperAPI.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)
