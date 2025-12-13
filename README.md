# Web Scraping with NewsAPI.org

![Web Scraping with newsAPI](web-scraping-newsapi.org.webp)

A comprehensive Python tutorial demonstrating how to gather news articles using the NewsAPI.org service. This guide walks you through the process of fetching news data, processing it, and converting it into structured formats like CSV files.

## Table of Contents

-   [Overview](#overview)
-   [Prerequisites](#prerequisites)
-   [Installation](#installation)
-   [Getting Started](#getting-started)
    -   [Get API Key](#get-api-key)
    -   [Import Libraries](#import-libraries)
-   [API Configuration](#api-configuration)
    -   [API Key Setup](#api-key-setup)
    -   [API Endpoints](#api-endpoints)
    -   [Request Parameters](#request-parameters)
-   [Making API Requests](#making-api-requests)
-   [Processing Responses](#processing-responses)
-   [Saving Data](#saving-data)
-   [Resources](#resources)

## Overview

Web scraping is a technique employed to gather large amounts of data from websites, whereby the data is extracted and stored in a structured format. This technique is widely used for a range of purposes, including market analysis, machine learning, news tracking, and financial data aggregation. The ability to perform web scraping is an important skill for any data scientist to have in their toolkit.

There are many different approaches for getting data from the web, such as writing a custom crawler from scratch, or by using one of the many existing libraries that make it easy to create a web scraping tool in Python.

In this Python tutorial, we will go through a simple example of how to scrape a website to gather news articles and create simple applications using the NewsAPI library.

## Prerequisites

-   Python 3.6 or higher
-   Basic knowledge of Python programming
-   Understanding of REST APIs and JSON data format

## Installation

Install the required Python libraries using pip:

```bash
pip install pandas requests
```

The libraries used in this tutorial are:

-   **pandas** - For data manipulation and analysis
-   **requests** - For making HTTP requests to the API
-   **json** - Built-in Python library for JSON handling

## Getting Started

### Get API Key

To interact with the NewsAPI service, you need to obtain a free API key:

1. Visit [NewsAPI.org](https://newsapi.org/)
2. Register for a free account (individual registration)
3. Navigate to your dashboard to retrieve your API key
4. The API key will be sent to your email shortly after registration

**Note:** The free tier provides access to breaking news headlines and limited article searches. For production use, consider upgrading to a paid plan.

### Import Libraries

Start by importing the necessary libraries:

```python
import pandas as pd
import requests
import json
```

## API Configuration

### API Key Setup

In order to interact with the API, it is mandatory to provide the API key. There are two common ways to authenticate:

1. **Using API key in the request URL:**

    ```
    https://newsapi.org/v2/everything?q=bitcoin&apiKey=YOUR_API_KEY
    ```

2. **Using headers (recommended):**
    ```python
    headers = {'Authorization': 'YOUR_API_KEY'}
    ```

**Security Note:** Never commit your API key to version control. Consider using environment variables or a configuration file that is excluded from git.

```python
# Create a variable to hold the API key
api_key = "YOUR_API_KEY_HERE"
headers = {'Authorization': api_key}
```

### API Endpoints

NewsAPI provides two main endpoints:

-   **[Everything](https://newsapi.org/docs/endpoints/everything)** `/v2/everything` – Search every article published by over 75,000 different sources large and small in the last 3 years. This endpoint is ideal for news analysis and article discovery.

-   **[Top Headlines](https://newsapi.org/docs/endpoints/top-headlines)** `/v2/top-headlines` – Returns breaking news headlines for countries, categories, and singular publishers. This is perfect for use with news tickers or anywhere you want to display live up-to-date news headlines.

```python
# Create variables to hold the API endpoints
everything = "https://newsapi.org/v2/everything?"
top_headlines = "https://newsapi.org/v2/top-headlines?"
```

### Request Parameters

#### Keywords

The `q` parameter stands for the keyword search value. News articles matching the keyword search are returned as a response.

```python
# In our tutorial, we look for news related to Apple
keywords = "apple"
```

#### Sources

A comma-separated string of identifiers for the news sources or blogs you want headlines from.

```python
sources = ['abc-news', 'business-insider', 'financial-post', 'google-news',
           'reuters', 'nbc-news', 'techcrunch', 'the-wall-street-journal']
```

#### Sort By

The `sortBy` parameter determines how the returned articles are sorted in the response.

**Possible options:** `relevancy`, `popularity`, `publishedAt`

```python
sort_by = "popularity"
```

#### Payloads

Create a payload dictionary containing the parameters to be sent to the API. The payload can include information such as news category, country, language, etc.

```python
params = {
    'q': keywords,
    'apiKey': api_key,
    'sortBy': sort_by,
    'language': 'en',
    'page': 1
}
```

## Making API Requests

Make the request using the `get()` method of the requests library. Pass the NewsAPI endpoint as the `url` parameter and the payloads dictionary to the `params` parameter.

```python
response = requests.get(url=top_headlines, headers=headers, params=params)
```

**Note:** The structure of the request remains the same throughout. You can update the `url` and `params` parameters to gather different types of news.

## Processing Responses

Create a variable to hold the response in dictionary format:

```python
output = response.json()
```

Print out the response on the console using the `json.dumps()` method for formatted output:

```python
print(json.dumps(output, indent=4))
```

**Example Response:**

```json
{
    "status": "ok",
    "totalResults": 10,
    "articles": [
        {
            "source": {
                "id": null,
                "name": "Rappler"
            },
            "author": "Rappler.com",
            "title": "Jollibee introduces new sweet chili chicken - Rappler",
            "description": "Jollibee's fried chicken is coated in a sweet-spicy chili glaze",
            "url": "https://www.rappler.com/life-and-style/food-drinks/jollibee-sweet-chili-chicken",
            "urlToImage": "https://assets2.rappler.com/2021/06/1-1623468746894.jpg",
            "publishedAt": "2021-06-12T03:39:00Z",
            "content": "Jollibee's new limited edition item can be ordered for dine-in, take-out, drive-thru, or delivery via the Jollibee Delivery app, website, hotline number, GrabFood, and foodpanda. Rappler.com"
        },
        {
            "source": {
                "id": null,
                "name": "Rappler"
            },
            "author": "Reuters",
            "title": "Hong Kong democracy activist Agnes Chow released from prison - Rappler",
            "description": "(1st UPDATE) Agnes Chow serves nearly seven months in prison for her role in an unauthorized assembly during Hong Kong's 2019 anti-government protests",
            "url": "https://www.rappler.com/world/asia-pacific/hong-kong-democracy-activist-agnes-chow-released",
            "urlToImage": "https://assets2.rappler.com/2021/06/Agnes-Chow-Wikimedia-June-12-2021-1623465814534.jpeg",
            "publishedAt": "2021-06-12T02:45:00Z",
            "content": "Hong Kong pro-democracy activist Agnes Chow was released from prison on Saturday, June 12, after serving nearly seven months for her role in an unauthorized assembly during anti-government protests i… [+18 chars]"
        }
    ]
}
```

## Saving Data

### Creating a DataFrame

To create a DataFrame with the results obtained, access the dictionary values using the "articles" key:

```python
# Check the available keys in the response
print(output.keys())
# Output: dict_keys(['status', 'totalResults', 'articles'])

# Extract articles
articles = output['articles']

# Create a pandas DataFrame
df = pd.DataFrame(articles)
df.head(3)
```

### Exporting to CSV

Save the DataFrame to a CSV file:

```python
df.to_csv('news_articles.csv', index=False)
```

**Example DataFrame Output:**

![DataFrame Preview](df.png)

## Resources

-   **Jupyter Notebook:** View the complete tutorial notebook [here](https://github.com/akladyous/newsapi-web-scraping)
-   **NewsAPI Documentation:** [https://newsapi.org/docs](https://newsapi.org/docs)
-   **Pandas Documentation:** [https://pandas.pydata.org/docs/](https://pandas.pydata.org/docs/)
-   **Requests Documentation:** [https://requests.readthedocs.io/](https://requests.readthedocs.io/)

---

Thanks for reading and happy web scraping! 🚀

For questions or contributions, please visit the [GitHub repository](https://github.com/akladyous/newsapi-web-scraping).
