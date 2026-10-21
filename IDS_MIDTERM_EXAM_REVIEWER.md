# IDS — MIDTERM EXAM REVIEWER
## Web Scraping (Beautiful Soup / Scrapy) & Social Media Data Extraction
## Ethics in Data Collection, Data Analysis, and Visualization

> **Source rule:** Everything here comes from the two slide decks. If a slide gives a numbered list (3 steps, 4 benefits, 8 checklist items), the exam wants that exact list.

---

# HOW TO USE THIS REVIEWER

| Test Type | How to Answer | Where to Drill |
|---|---|---|
| **Modified Multiple Choice** | Watch for "All of the above / Both A and B / Which is NOT." Eliminate two choices first. | Drill A per part |
| **Modified True or False** | If FALSE, state the correction. Memorize trap words: *always, only, automatically, guarantees, public = permitted*. | Drill B per part |
| **Enumeration / Exemplification** | Give the exact items from the slides, with a one-line explanation each. | Drill C per part |
| **Essay** | Use the model outlines. One idea per bullet, then expand each in 1–2 sentences. | Drill D per part |
| **Code Tracing / Illustration** | Name each component in order, then explain what it does. | Drill E per part |

**Golden rules:**
1. **Public visibility ≠ permission** for automated collection.
2. **API first**, scraping only when permitted.
3. **Collect only what the research question requires.**
4. **Document source, method, limitations, and ethical decisions.**

---
---

# PART A — WEB SCRAPING & SOCIAL MEDIA DATA EXTRACTION

---

# MODULE A1: FOUNDATIONS OF WEB DATA EXTRACTION

## A1.1 Core Definitions

| Term | Definition |
|---|---|
| **Web scraping** | The **automated process of retrieving selected information from webpages**. A program requests a webpage, examines its HTML structure, and identifies the elements containing the required information. Extracted information is converted into **structured records** for analysis. |
| **Web crawling** | **Systematically follows links** to discover and process multiple connected pages. |
| **API extraction** | Requests **structured information directly from an authorized application interface**. |
| **Structured record** | Data organized into consistent fields (e.g., title, date, category) suitable for analysis. |
| **Data acquisition** | The stage of the data science lifecycle where data is collected. Web extraction belongs here. |
| **Research question** | The clear question that determines which variables and sources are relevant. Extraction must begin with one. |
| **Variable** | A specific field/attribute required to answer the research question. |

## A1.2 Why Web Data Matters

- Websites contain **large amounts of information** that may support research, decision-making, and predictive analysis.
- **Manual copying** from hundreds of pages is **slow** and produces **inconsistent records**.
- **Automated extraction** is **efficient and reproducible**.
- Quality of the final analysis depends on whether collected information is **relevant, complete, accurate, and ethically obtained**.

## A1.3 Tool Selection (memorize this table)

| Situation | Appropriate Method |
|---|---|
| Small or moderately sized **static** webpages | **Beautiful Soup** |
| Multiple connected and **paginated** webpages | **Scrapy** |
| Structured platform or **social-media** information | **Official API** |

## A1.4 Practical Applications

Price monitoring, content analysis, public-record collection, market research, article metadata, job listings, product information, environmental records, disaster-advisory titles and dates.

- The selected application must have a **legitimate purpose** and an **appropriate public or authorized source**.

## A1.5 General Extraction Pipeline (order matters)

**Research Question → Source Assessment → Permission Check → Data Retrieval → Parsing → Cleaning → Validation → Storage → Documentation**

- The pipeline **begins** by defining the problem and evaluating the proposed source.
- It **ends** by documenting source, method, limitations, and ethical decisions.

---

# MODULE A2: WEBSITES AND HTML

## A2.1 Anatomy of a URL

| Component | Meaning | Example |
|---|---|---|
| **Scheme** | Protocol used | `https` |
| **Domain** | Server/host name | `example.com` |
| **Path** | Resource location | `/articles` |
| **Query parameters** | Key–value data sent to the server | `page=2` |
| **Fragment** | Location within the page | `results` |

Example: `https://example.com/articles?page=2#results`

- Understanding URL parts helps a scraper **construct requests** and **follow related pages** correctly.

## A2.2 Client–Server Communication

- **Client** — application that **requests** information from another computer or service (browser, Python script).
- **Server** — receives the request, **processes** it, and returns an appropriate **response**.
- Example: A Python program requests a webpage; the website server returns its **HTML content**.

## A2.3 HTTP Requests and Responses

| Message | Contains |
|---|---|
| **HTTP request** | Target address, request **method**, headers, other request information |
| **HTTP response** | **Status code**, headers, requested content (when successful) |

- A scraper must **inspect the response** before attempting to parse its content.

| Status Code | Meaning |
|---|---|
| **200** | Successful request |
| **403** | Access forbidden |
| **404** | Resource not found |
| **429** | Too many requests |
| **500** | Server error |

## A2.4 HTML Structure

- HTML organizes webpage content using **elements** represented by **opening and closing tags**.
- Elements contain **text**, **other elements**, and **attributes** describing identity or behavior.
- **Tags** identify the general function (paragraph, heading, image, link).
- **Classes** group related elements; **IDs** uniquely identify an element.
- **Attributes** provide extra information (link destination, image source, record identifier).

```html
<a class="article-link" id="article-101" href="/articles/101">Read Article</a>
```

## A2.5 Document Object Model (DOM)

- The DOM represents the document as a **hierarchical tree**.
- **Parent** elements contain **children**; children may contain **descendants**.
- Relationships: **parent, child, sibling, descendant**.
- Beautiful Soup and Scrapy use this structure to locate and extract information.

```
html
├── head
│   └── title
└── body
    ├── h1
    └── div
        ├── span
        └── a
```

## A2.6 CSS Selectors

| Selector | Meaning |
|---|---|
| `div` | Any `div` element |
| `div.article` | A `div` with class `article` |
| `span.title` | A `span` with class `title` |
| `#article-101` | The element with the specified ID |
| `a[href]` | Link attribute (Beautiful Soup CSS) |
| `a::attr(href)` | Link attribute (Scrapy CSS) |

- CSS selectors provide a **concise way** to identify elements by tag, class, ID, or relationship.
- A reliable selector identifies the required element **without depending unnecessarily on unrelated page formatting**.

## A2.7 Static vs Dynamic Pages

| Page Type | Behavior | Evidence | Appropriate Method |
|---|---|---|---|
| **Static** | Main content is inside the **original HTML response** | Product names and prices appear in original HTML | Beautiful Soup or Scrapy |
| **Dynamic** | JavaScript retrieves or generates content **after initial load** | Records appear only after JavaScript runs | Official API or authorized JSON endpoint |

- Some information visible in the browser may be **absent from the original HTML**.
- Before collecting, inspect the page source and **Fetch/XHR requests** to determine the appropriate and permitted method.

---

# MODULE A3: ETHICAL, LEGAL, AND RESPONSIBLE EXTRACTION

## A3.1 Permission Comes Before Collection

- **Public visibility does not automatically mean automated collection is unrestricted.**
- Source evaluation examines **purpose, policies, and nature** of the requested information **before** scraper development.
- Collection must **stop** when the website prohibits access or when proper authorization is unavailable.

## A3.2 robots.txt

- A website may publish **`robots.txt`** to communicate instructions to automated crawlers.
- It identifies paths that crawlers are **allowed** or **discouraged** from visiting.
- These instructions must be considered **together with** platform terms, privacy requirements, and explicit authorization.

```python
from urllib.robotparser import RobotFileParser

robots = RobotFileParser()
robots.set_url("https://example.com/robots.txt")
robots.read()
allowed = robots.can_fetch(
    "UNO-R-DataScience-Class/1.0",
    "https://example.com/public/"
)
print("Allowed:", allowed)
```

> `robots.txt` is **not** the only rule. It does **not** grant ethical or legal permission by itself.

## A3.3 Use Official APIs When Available

- An official API provides a **structured, authorized** method for requesting available fields.
- It normally defines **authentication procedures, request parameters, and usage limitations**.
- An API should generally be **preferred** when a platform provides one.
- Example: use the **YouTube Data API** for authorized public video metadata instead of scraping the YouTube interface.

## A3.4 Privacy and Data Minimization

- **Data minimization** = collecting **only the information required** by the research question.
- Do **not** collect personal identifiers merely because they are visible or technically accessible.
- Sensitive or unnecessary information should be **excluded, anonymized, or protected**.
- Example: collect publication date and topic of a public post, **not** the profile photo or personal biography.

## A3.5 Responsible Request Frequency

- Too many requests in a short period may **overload a server** or **activate protective controls**.
- A responsible scraper:
  - limits the number of pages,
  - introduces **delays**,
  - observes **server responses**,
  - slows down or stops when the server reports **excessive activity (429)**.

```python
import time
for page_number in range(1, 6):
    print("Collecting page:", page_number)
    time.sleep(1)
```

## A3.6 Copyright and Data Provenance

- Webpage content may remain **protected by copyright** even when publicly accessible.
- **Data provenance** records **where, when, and how** each record was collected.
- Proper documentation lets researchers evaluate the **reliability** and **limitations** of the dataset.

```python
record = {
    "source_url": "https://example.com/articles",
    "collection_method": "Beautiful Soup",
    "collected_at": "2026-09-03T08:00:00Z"
}
```

## A3.7 Pre-Collection Checklist (8 items)

1. Public or properly authorized source
2. Legitimate research purpose
3. Necessary variables only
4. Reasonable page limit
5. Appropriate request delay
6. No access-control bypass
7. Data-protection plan
8. Complete source documentation

---

# MODULE A4: PREPARING THE PYTHON ENVIRONMENT

## A4.1 Virtual Environment

- A **virtual environment** creates an **isolated workspace** with its own Python interpreter and installed packages.
- It prevents **package-version conflicts** and makes the project **reproducible** across computers.
- Created once per project, **activated before development**, deactivated when the session ends.

```bash
python -m venv scraping_env
# Windows PowerShell
.venv\Scripts\Activate.ps1
```

## A4.2 Required Libraries

| Library | Purpose |
|---|---|
| **requests** | Retrieves webpage and API content through HTTP |
| **Beautiful Soup (`bs4`)** | Parses HTML/XML |
| **lxml** | Fast parser backend |
| **pandas** | Organizes extracted records into tables |
| **Scrapy** | Complete framework for structured multi-page crawling |

```bash
pip install requests beautifulsoup4 lxml pandas scrapy
```

## A4.3 Project Structure

```
web_data_project/
├── data/
│   ├── raw/
│   └── processed/
├── logs/
├── notebooks/
├── spiders/
├── src/
├── .env
├── requirements.txt
└── README.md
```

- Separate **raw data, processed data, source code, logs, configuration, documentation**.
- **Preserve raw data** so every cleaning/transformation step is traceable and reproducible.
- Store **credentials** in `.env`, never in shared code or public repositories.

## A4.4 Verifying the Environment

```python
import requests
import pandas as pd
import scrapy
from bs4 import BeautifulSoup

print("Environment ready.")
```

- Successful imports confirm Python can locate the packages and dependencies.
- Version checks and resolved imports establish a stable foundation before extraction.

---

# MODULE A5: WEB SCRAPING WITH BEAUTIFUL SOUP

## A5.1 Sending the First Request

```python
import requests

url = "https://quotes.toscrape.com/"
response = requests.get(url, timeout=15)
print(response.status_code)
```

- `requests.get()` retrieves content from a specified address.
- **`timeout`** prevents the program from waiting indefinitely when the server does not respond.
- The **response object** contains status, headers, final address, and webpage content.

## A5.2 Checking the Response Safely

```python
import requests

try:
    response = requests.get("https://quotes.toscrape.com/", timeout=15)
    response.raise_for_status()
    print("Request successful.")
except requests.RequestException as error:
    print("Request failed:", error)
```

- **`raise_for_status()`** raises an exception for unsuccessful HTTP responses.
- Request errors should be **recorded and handled**, not silently ignored.

## A5.3 Creating the Beautiful Soup Object

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(response.text, "html.parser")
print(soup.title.get_text(strip=True))
```

- Beautiful Soup converts returned HTML text into a **searchable Python object**.
- The **parser** determines how HTML is interpreted and organized.
- The resulting object allows the program to **navigate, search, and extract** elements.

## A5.4 Inspecting the Parsed Document

```python
print(soup.prettify()[:1000])
```

- **`prettify()`** displays parsed HTML as an **organized, indented tree**.
- It reveals **parent, child, sibling, and descendant** relationships.
- Print only a limited portion to keep inspection readable.

## A5.5 Finding Elements

| Method | Returns |
|---|---|
| `find()` | The **first** element satisfying the condition |
| `find_all()` | **Every** matching element as a collection |
| `select_one()` | The **first** element matching a CSS selector |
| `select()` | **All** elements matching a CSS selector |

```python
first_author = soup.find("small", class_="author")
all_authors = soup.find_all("small", class_="author")

first_quote = soup.select_one("div.quote")
all_quotes = soup.select("div.quote")
```

## A5.6 Extracting Text and Attributes

```python
text = first_quote.select_one("span.text").get_text(strip=True)
link = first_quote.select_one("a").get("href")
```

- **`get_text(strip=True)`** extracts text with whitespace removal.
- Attributes can be accessed through **dictionary-style indexing** or **`get()`**.
- Attribute extraction is commonly used for links, IDs, image sources, and categories.

## A5.7 Extracting Repeated Records

```python
records = []
for quote in soup.select("div.quote"):
    records.append({
        "quote": quote.select_one("span.text").get_text(strip=True),
        "author": quote.select_one("small.author").get_text(strip=True),
        "tags": [tag.get_text(strip=True) for tag in quote.select("a.tag")]
    })
```

- Repeated records normally share the **same container structure**.
- Iterate through containers and extract the **same fields** from each.
- Use **consistent field names** for every record.

## A5.8 Handling Missing Elements (defensive extraction)

```python
def safe_text(container, selector):
    element = container.select_one(selector)
    if element is None:
        return None
    return element.get_text(strip=True)
```

- A record may be **incomplete** or structured differently.
- Calling a method on a **missing element** can crash the program.
- Check whether an element exists **before** retrieving its content.

## A5.9 Extracting an HTML Table

```python
table = soup.select_one("table")
headers = [h.get_text(strip=True) for h in table.select("th")]

rows = []
for row in table.select("tr"):
    values = [cell.get_text(strip=True) for cell in row.select("td")]
    if values:
        rows.append(dict(zip(headers, values)))
```

- Tables organize records into **rows and columns**.
- Distinguish the **header row** from data rows.
- Convert each row to a dictionary using headings as field names.

## A5.10 Converting Relative Links

```python
from urllib.parse import urljoin

complete_link = urljoin(
    "https://quotes.toscrape.com/",
    "/author/Albert-Einstein"
)
```

- A **relative link** depends on the current page address.
- **`urljoin()`** combines the current address and relative path into a complete URL.
- Prevents later requests from using incomplete or invalid addresses.

## A5.11 Detecting Pagination

```python
next_link = soup.select_one("li.next a")
if next_link:
    next_url = urljoin(response.url, next_link.get("href"))
else:
    next_url = None
```

- **Pagination** divides records across several webpages.
- Identify the **next-page link** and stop when no additional page exists.
- Include a **maximum-page limit** to prevent uncontrolled collection.

## A5.12 Collecting Multiple Pages

```python
import time

url = "https://quotes.toscrape.com/"
page_number = 1
maximum_pages = 5
records = []

while url and page_number <= maximum_pages:
    response = requests.get(url, timeout=15)
    response.raise_for_status()
    soup = BeautifulSoup(response.text, "html.parser")

    for quote in soup.select("div.quote"):
        records.append({
            "quote": safe_text(quote, "span.text"),
            "author": safe_text(quote, "small.author"),
            "page": page_number,
            "source_url": url
        })

    next_link = soup.select_one("li.next a")
    url = urljoin(url, next_link.get("href")) if next_link else None
    page_number += 1
    time.sleep(1)
```

- A multi-page scraper repeats **request → parsing → extraction** for every approved page.
- Include a **request delay, collection timestamp, and page counter**.
- Terminate when the next-page link disappears **or** the approved page limit is reached.

## A5.13 Creating and Exporting the Dataset

```python
import pandas as pd

df = pd.DataFrame(records)
print(df.head())
print(df.info())
df.to_csv("data/raw/quotes_raw.csv", index=False)
```

- Pandas converts a list of dictionaries into a **tabular DataFrame**.
- Inspect for **missing fields, duplicates, inconsistent values** before analysis.
- Export the **raw version** without overwriting the original collection.

---

# MODULE A6: MULTI-PAGE CRAWLING WITH SCRAPY

## A6.1 When Scrapy Becomes Appropriate

- Beautiful Soup is appropriate for **controlled extraction from small/moderate static pages**.
- Scrapy is designed for projects with **multiple pages, scheduled requests, pagination, pipelines, logging, and structured exports**.
- Tool selection depends on **source structure, collection scale, technical complexity, and reproducibility requirements**.

## A6.2 Beautiful Soup vs Scrapy

| Requirement | Beautiful Soup | Scrapy |
|---|---|---|
| Small static page | **Excellent** | Possible |
| Multi-page crawling | **Manual** | **Built in** |
| Request scheduling | **Manual** | **Built in** |
| Pipelines | Custom | **Built in** |
| Logging and statistics | Custom | **Built in** |

- Beautiful Soup is primarily an **HTML-parsing library**, commonly combined with `requests`.
- Scrapy is a **crawling framework** managing requests, responses, callbacks, extraction, and item processing.
- Both can use CSS selectors; Scrapy provides more **built-in project infrastructure**.

## A6.3 Scrapy Architecture

**Spider → Request → Downloader → Response → Parser → Item Pipeline → Dataset**

- A **spider** generates requests and processes returned responses.
- The **downloader** fetches pages.
- The **parser/callback** extracts items.
- **Item pipelines** clean, validate, or store items.
- **Settings** control request behavior, delays, retries, concurrency, logging, and other operations.

## A6.4 Creating a Scrapy Project

```bash
scrapy startproject quoteproject
cd quoteproject
scrapy genspider quotes quotes.toscrape.com
```

- `startproject` generates standard folders and configuration files.
- `genspider` creates an initial spider for a specified **approved domain**.
- Keep the project inside a dedicated workspace.

## A6.5 Scrapy Project Structure

```
quoteproject/
├── scrapy.cfg
└── quoteproject/
    ├── items.py
    ├── pipelines.py
    ├── settings.py
    └── spiders/
        └── quotes.py
```

| File/Folder | Purpose |
|---|---|
| `spiders/` | Classes defining crawling and extraction behavior |
| `items.py` | Item/field definitions |
| `pipelines.py` | Post-extraction cleaning and validation |
| `settings.py` | Project-wide behavior and enabled components |
| `scrapy.cfg` | Project configuration entry point |

## A6.6 Anatomy of a Spider

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"
    allowed_domains = ["quotes.toscrape.com"]
    start_urls = ["https://quotes.toscrape.com/"]
```

- A spider is a Python class inheriting from **`scrapy.Spider`**.
- It normally defines **`name`**, **`allowed_domains`**, **`start_urls`**, and one or more **parsing methods**.
- It should describe a **specific, limited collection purpose**.

## A6.7 Starting Requests and the parse() Callback

```python
def parse(self, response):
    page_title = response.css("title::text").get()
    self.logger.info("Processing: %s", page_title)
```

- **`start_urls`** tell Scrapy where crawling begins.
- Scrapy creates requests for these locations and sends successful responses to the default parsing method.
- **`parse()`** receives a response and decides how content is processed.
- It may **extract records**, **generate additional requests**, or **both**.

## A6.8 Scrapy Shell

```bash
scrapy shell https://quotes.toscrape.com/
```

```python
response.status
response.css("title::text").get()
response.css("div.quote").get()
response.css("small.author::text").getall()
```

- Provides an **interactive environment** for examining responses and testing selectors without running the whole spider.
- CSS and XPath expressions are evaluated immediately against the HTML.
- Verified selectors reduce development time and isolate extraction errors.

## A6.9 CSS Selectors in Scrapy

```python
text = quote.css("span.text::text").get()
tags = quote.css("a.tag::text").getall()
```

| Syntax | Meaning |
|---|---|
| `::text` | Extract textual content |
| `::attr(name)` | Extract a specified attribute |
| `.get()` | Retrieve **one** match |
| `.getall()` | Retrieve **all** matches |

## A6.10 XPath Selectors in Scrapy

```python
authors = response.xpath("//small[@class='author']/text()").getall()
links = response.xpath("//li[@class='next']/a/@href").get()
```

- XPath locates elements by **position, attributes, content, and relationships** in the document tree.
- **Predicates and path expressions** support more complex conditions than basic CSS.
- Basic XPath complements CSS when structures need more precise navigation.

## A6.11 Returning Structured Records

```python
for quote in response.css("div.quote"):
    yield {
        "quote": quote.css("span.text::text").get(),
        "author": quote.css("small.author::text").get(),
        "tags": quote.css("a.tag::text").getall(),
        "source_url": response.url
    }
```

- **`yield`** returns an extracted record to Scrapy **without ending the crawl**.
- Each record should use the **same field names** and compatible data types.
- Consistent fields make later cleaning, validation, and analysis easier.

## A6.12 Following Links and Pagination

```python
author_link = quote.css("a::attr(href)").get()
yield response.follow(author_link, callback=self.parse_author)
```

```python
next_page = response.css("li.next a::attr(href)").get()
if next_page:
    yield response.follow(next_page, callback=self.parse)
```

- **`response.follow()`** creates a new request using a discovered link.
- It can resolve **relative paths** and send the resulting response to a selected callback.
- Follow only links **inside the approved collection scope**.
- A paginated spider follows the next-page link and sends the response back to the **same parsing method**.
- Crawling ends naturally when the next-page element is gone.

## A6.13 Cleaning with an Item Pipeline

```python
class CleanQuotePipeline:
    def process_item(self, item, spider):
        if item.get("quote"):
            item["quote"] = " ".join(item["quote"].split())
        if item.get("author"):
            item["author"] = item["author"].strip()
        item["tags"] = ", ".join(item.get("tags", []))
        return item
```

- A pipeline processes extracted records **after they leave the spider**.
- It may **standardize text, validate required fields, remove invalid records, or prepare for storage**.
- Keeping cleaning logic in a pipeline prevents the spider from becoming unnecessarily complicated.

## A6.14 Exporting Results

```bash
scrapy crawl quotes -O quotes.csv
scrapy crawl quotes -o quotes.jsonl
```

| Option | Behavior |
|---|---|
| **`-O` (uppercase)** | **Replaces** an existing output file |
| **`-o` (lowercase)** | Appends/outputs to file (does not overwrite the same way) |

- Formats: **CSV, JSON, JSON Lines (JSONL)**.
- Exported records still require **schema, missing-value, and duplicate checks** before they are analysis-ready.

## A6.15 Responsible Scrapy Settings

```python
ROBOTSTXT_OBEY = True
DOWNLOAD_DELAY = 1.0
AUTOTHROTTLE_ENABLED = True
CONCURRENT_REQUESTS_PER_DOMAIN = 8
RETRY_TIMES = 2
LOG_LEVEL = "INFO"
CLOSESPIDER_PAGECOUNT = 10
```

| Setting | Purpose |
|---|---|
| `ROBOTSTXT_OBEY` | Respect `robots.txt` |
| `DOWNLOAD_DELAY` | Delay between requests |
| `AUTOTHROTTLE_ENABLED` | Automatically adjust request rate |
| `CONCURRENT_REQUESTS_PER_DOMAIN` | Limit simultaneous requests per domain |
| `RETRY_TIMES` | Retry temporary failures (not to bypass restrictions) |
| `LOG_LEVEL` | Control logging verbosity |
| `CLOSESPIDER_PAGECOUNT` | Stop after a page limit |

- Logs record processed pages, response codes, failed requests, extracted items, and why a spider stopped.
- **Retries must not be used to overcome deliberate access restrictions.**
- Crawl statistics summarize request counts, response results, processing time, and item totals for validation.

---

# MODULE A7: SOCIAL MEDIA DATA EXTRACTION

## A7.1 What Is Social Media Data Extraction?

- Collects **selected platform information for a defined analytical purpose**.
- Possible fields: **public post metadata, publication dates, topics, engagement counts, permitted comments**.
- Collection must follow **platform policies, privacy requirements, and the conditions of the authorized interface**.

## A7.2 API-First Approach

- Social-media interfaces are frequently **dynamic, restricted, and protected** by platform-specific controls.
- An official API provides a **structured and accountable** method for requesting approved information.
- Attempts to bypass **CAPTCHAs, authentication, private accounts, or API limitations** violate responsible data-collection practices.

**Preferred order:**
1. Official API
2. Authorized institutional dataset
3. Instructor-provided JSON
4. HTML scraping **only when explicitly permitted**

## A7.3 Public vs Private Information

| Classification | Required Handling |
|---|---|
| **Public metadata** | Collect only when permitted and necessary |
| **Private content** | Do not collect |
| **Sensitive personal data** | Exclude unless explicitly authorized |
| **Deleted or restricted content** | Do not attempt to recover |

- **Public** = accessible without special authorization.
- **Private/restricted** = limited to approved users or applications (private messages, hidden profiles, deleted posts, access-controlled content).
- **Technical accessibility alone does not establish ethical or legal permission.**

## A7.4 API Keys and OAuth

| Term | Meaning |
|---|---|
| **API key** | Identifies the **application** making a request |
| **OAuth** | Authorizes access under **defined user permissions** |

- Credentials must be stored **securely** and excluded from shared code, screenshots, and repositories.
- A compromised credential must be **revoked and replaced immediately**.
- Use an **environment variable**; never display the key.

```python
import os
api_key = os.environ.get("YOUTUBE_API_KEY")
if not api_key:
    raise RuntimeError("API key is not configured.")
```

## A7.5 Endpoints and Query Parameters

- **Endpoint** = specific API resource or operation available to an application.
- **Query parameters** define requested fields, search term, result limit, resource type, and pagination information.

```python
endpoint = "https://www.googleapis.com/youtube/v3/search"
parameters = {
    "part": "snippet",
    "q": "data science Philippines",
    "type": "video",
    "maxResults": 10,
    "key": api_key
}
```

- **Data minimization** restricts each request to the fields and records necessary for the research question.

## A7.6 Understanding JSON Responses

- APIs commonly return **JavaScript Object Notation (JSON)**.
- A JSON response may contain **nested objects, arrays, result metadata, and pagination tokens**.
- Inspect the structure before selecting required fields.

```json
{
  "nextPageToken": "TOKEN_VALUE",
  "items": [
    {
      "id": { "videoId": "abc123" },
      "snippet": {
        "title": "Introduction to Data Science",
        "publishedAt": "2026-08-20T08:00:00Z"
      }
    }
  ]
}
```

## A7.7 API Pagination

- An API may divide a large result set into several **response pages**.
- A **page token** identifies the next group of available results without requiring the application to guess a webpage address.
- Use a **maximum-page limit** and stop when no next token is returned.

```python
page_token = payload.get("nextPageToken")
if page_token:
    parameters["pageToken"] = page_token
```

## A7.8 Quotas and Rate Limits

- **API quotas** limit the number of requests or **cost units** an application may consume during a defined period.
- Monitor request counts, response codes, page totals, and returned quota information.
- Collection must **slow down or stop** when an established quota or rate limit is reached.

## A7.9 Data Dictionary (field selection)

- A data dictionary defines every variable by **name, data type, source, format, meaning, and permitted use**.
- Preparing it **before collection** prevents unnecessary or inconsistent information from entering the dataset.
- **Required and optional fields** provide a clear basis for schema and completeness validation.

| Field | Type | Purpose |
|---|---|---|
| `video_id` | String | Unique public video reference |
| `title` | String | Content/topic analysis |
| `published_at` | Datetime | Temporal analysis |
| `search_query` | String | Collection context |
| `collected_at` | Datetime | Provenance |

## A7.10 Sending a YouTube API Request

```python
import requests

response = requests.get(endpoint, params=parameters, timeout=15)
response.raise_for_status()
payload = response.json()
```

- `requests` can send query parameters to an authorized API endpoint.
- Check the response **before** processing JSON.
- Get the API key from an **environment variable**; never display it.

## A7.11 Extracting API Records

```python
records = []
for item in payload.get("items", []):
    snippet = item.get("snippet", {})
    records.append({
        "video_id": item.get("id", {}).get("videoId"),
        "title": snippet.get("title"),
        "published_at": snippet.get("publishedAt")
    })
```

- Iterate through the **`items` array**.
- Required values may be inside **nested objects** such as `id` and `snippet`.
- Defensive **`.get()`** access prevents failure when an optional field is absent.

## A7.12 Creating the Social Media Dataset

```python
import pandas as pd

df = pd.DataFrame(records)
print(df.head())
print(df.shape)
print(df.isna().sum())
df.to_csv("data/raw/youtube_results.csv", index=False)
```

- Dataset inspection covers **dimensions, column names, data types, missing values, and duplicated identifiers**.
- Store the **original API response** and the **tabular processed dataset separately** to preserve provenance.

## A7.13 Anonymization and Pseudonymization

| Technique | Meaning | Key Point |
|---|---|---|
| **Anonymization** | Removes/transforms identifying information so individuals are not reasonably identifiable | Direct identifiers are not the only risk; indirect combinations can reveal identity |
| **Pseudonymization** | Replaces an identifier with a **coded value** | Re-identification risk may remain if the key exists |

```python
import hashlib

def create_code(identifier):
    return hashlib.sha256(identifier.encode("utf-8")).hexdigest()[:12]
```

- The safest approach is often to **avoid collecting the identifier entirely**.
- Direct identifiers should be excluded when not required by the study.

---

# MODULE A8: DATA QUALITY AND VALIDATION

## A8.1 Defining the Dataset Schema

```python
required_columns = {"video_id", "title", "published_at"}
missing_columns = required_columns - set(df.columns)
if missing_columns:
    raise ValueError(f"Missing columns: {missing_columns}")
```

- A **schema** specifies expected **fields and data types** for every record.
- It allows the program to detect **missing columns** and **incompatible values**.
- Derive the schema from the **data dictionary** and **research question**.

## A8.2 Handling Missing Values

```python
print(df.isna().sum())
df = df.dropna(subset=["video_id", "title"])
```

- Missing values occur when:
  - the source omits a field,
  - an API returns an optional value as absent,
  - a selector fails to locate an element.
- The **cause** determines whether a value should be retained, removed, corrected, or replaced.
- **Document every treatment decision** for transparency.

## A8.3 Detecting Duplicate Records

```python
duplicates = df.duplicated(subset=["video_id"]).sum()
print("Duplicates:", duplicates)
df = df.drop_duplicates(subset=["video_id"])
```

- Duplicates arise from **overlapping sources** or processing the **same page more than once**.
- Use a **stable identifier** to decide whether two records represent the same observation.
- Removing duplicates without defining comparison fields may remove **valid information**.

## A8.4 Cleaning Text and Encoding

```python
import html

df["title"] = (
    df["title"]
    .astype("string")
    .map(html.unescape)
    .str.replace(r"\s+", " ", regex=True)
    .str.strip()
)
```

- Extracted text may contain **repeated spaces, line breaks, markup remnants, or encoded characters**.
- Cleaning preserves meaningful content while removing formatting artifacts.
- Keep the **original text in the raw dataset**.

## A8.5 Standardizing Dates and Time Zones

```python
df["published_at"] = pd.to_datetime(
    df["published_at"],
    errors="coerce",
    utc=True
)
```

- Dates from different sources may use **different formats and time zones**.
- Convert valid strings into a consistent **datetime** representation.
- Invalid values become **missing values** for investigation (`errors="coerce"`) rather than being silently misread.

## A8.6 Validating and Preserving the Dataset

```python
quality_report = {
    "rows": len(df),
    "duplicates": int(df.duplicated().sum()),
    "missing": df.isna().sum().to_dict()
}
print(quality_report)
```

- Validation checks **schema, uniqueness, completeness, and value requirements**.
- Store **raw data, processed data, logs, and collection metadata separately**.
- This structure makes the transformation process reviewable and reproducible.

## A8.7 Selecting the Correct Tool

| Situation | Recommended Method |
|---|---|
| One static page | Beautiful Soup |
| Several paginated pages | Scrapy |
| Public platform metadata | Official API |
| Private or restricted content | Do not collect without authorization |

## A8.8 Common Problems and Responses

| Problem | Appropriate Response |
|---|---|
| Access denied / forbidden | Stop and review access conditions |
| Missing page | Verify and document the missing page |
| Too many requests (429) | Stop or reduce requests |
| Empty selector | Reinspect the HTML |
| Duplicate records | Apply a stable identifier |
| Dynamic content | Look for an authorized API |
| Exposed API key | Revoke and replace the credential |

- Extraction failures may result from **incorrect selectors, modified HTML, unavailable pages, request limitations, encoding problems, or missing fields**.
- Systematic diagnosis uses **logs, page inspection, response codes, and validation results**.
- Authentication requirements and access restrictions are **collection boundaries**, not technical obstacles to bypass.

## A8.9 Key Takeaways

- Responsible extraction begins with a **legitimate question** and an **appropriate information source**.
- Beautiful Soup, Scrapy, and APIs provide **different methods** for retrieving and structuring permitted information.
- The resulting records must still be **cleaned, validated, protected, documented, and reviewed** before analysis.

---
---

# PART B — ETHICS IN DATA COLLECTION, DATA ANALYSIS, AND VISUALIZATION

---

# MODULE B1: FOUNDATIONS OF DATA ETHICS

## B1.1 Why Ethics Matters

- Data science influences decisions involving **education, employment, healthcare, public services, and financial opportunities**.
- Poorly designed practices may:
  - expose private information,
  - reinforce discrimination,
  - produce misleading conclusions.
- Ethical practice ensures data-driven decisions **respect people** while delivering meaningful benefits.
- Example: a credit-scoring model may speed up loan processing but unfairly disadvantage groups underrepresented in its training data.

## B1.2 Meaning of Data Ethics

- **Data ethics** examines the **moral responsibilities** associated with **collecting, processing, analyzing, and communicating data**.
- It considers how data practices affect **individuals, communities, institutions, and society**.
- Its purpose is to promote responsible decisions **beyond technical accuracy and legal compliance**.
- Example: a facial recognition system may be technically accurate overall but ethically unacceptable if it performs poorly for particular demographic groups.

## B1.3 Ethics vs Legal Compliance

| Concept | Focus |
|---|---|
| **Legal compliance** | Whether a practice follows applicable **laws and regulations** |
| **Ethical evaluation** | Also considers **fairness, transparency, human dignity, and possible social harm** |

- A **legally permitted** activity may still be **irresponsible** when it violates reasonable expectations or creates unnecessary risks.
- Example: a company legally purchases customer profiles from a data broker, but customers never expected their information to be used for employee screening.

## B1.4 Core Ethical Values

1. **Honesty**
2. **Fairness**
3. **Privacy**
4. **Accountability**
5. **Transparency**
6. **Avoidance of harm**

- These guide choices when **technical objectives conflict with human interests**.
- They also support **public confidence** in data-driven systems.

## B1.5 Balancing Benefits and Possible Harm

- Ethical evaluation compares **expected benefits** with **possible harms**.
- Harms may include:
  - privacy loss,
  - discrimination,
  - financial exclusion,
  - reputational damage,
  - emotional distress.
- A project becomes more defensible when **risks are minimized** and **benefits are distributed fairly**.

## B1.6 Respect for Human Autonomy and Dignity

- **Human autonomy** recognizes an individual's ability to make **informed decisions** about participation and personal information.
- **Human dignity** requires people to be treated as **persons**, not merely as sources of data.
- Data practices must avoid **manipulation, humiliation, and unnecessary intrusion**.

## B1.7 Justice and Fair Treatment

- **Justice** concerns the fair distribution of a project's **benefits, risks, and burdens**.
- A population must not carry most of the risk while another receives most of the benefit.
- Fair treatment requires attention to groups that may be **excluded from data or decision-making**.
- Example: a medical dataset collected mainly from urban hospitals may not support equally reliable conclusions for rural communities.

## B1.8 Transparency

- Transparency makes the **purpose, methods, limitations, and consequences** of data processing understandable.
- It allows affected individuals to recognize how information contributes to decisions.
- Transparency does **not** require exposing confidential information or security-sensitive technical details.

## B1.9 Accountability

- **Accountability** assigns responsibility for the **design, operation, monitoring, and consequences** of a data system.
- It requires **documentation, oversight, and procedures for correcting errors**.
- **Automated processing does not remove human/institutional responsibility.**

## B1.10 Stakeholders

- **Data subjects**
- **Analysts**
- **Decision-makers**
- **Organizations**
- **Regulators**
- **Communities**

- Different stakeholders may experience **different benefits and risks** from the same project.
- Stakeholder analysis reveals concerns that a purely technical assessment may overlook.

## B1.11 Ethics across the Data Life Cycle

- Responsibilities begin **before collection** and continue through **storage, analysis, sharing, and deletion**.
- Decisions in one stage can create risks in later stages.
- Examples:
  - **Consent** during collection
  - **Access control** during storage
  - **Bias** during analysis
  - **Disclosure** during reporting

> Ethics is **not** a final review performed after the project is complete.

---

# MODULE B2: ETHICS IN DATA COLLECTION

## B2.1 Purpose Limitation

- Requires a **clear and specific reason** for collecting personal data.
- Information collected for one purpose must **not automatically be reused** for an unrelated purpose.
- Proposed secondary use requires a **valid basis and renewed ethical assessment**.

## B2.2 Necessity and Proportionality

| Principle | Question |
|---|---|
| **Necessity** | Can the objective be achieved **without** collecting this particular information? |
| **Proportionality** | Is the **amount and sensitivity** of data reasonable for the expected benefit? |

- Excessive collection creates risks that cannot be justified by minor convenience.
- Example: a precise home address is **disproportionate** when city-level location is sufficient.

## B2.3 Data Minimization

- Limits collection to information that is **relevant and necessary** for the stated purpose.
- Reduces **privacy exposure, security requirements, and possible misuse**.
- Do **not** collect unused variables merely because they might become useful later.

## B2.4 Collection Methods

- Surveys
- Interviews
- Observation
- Experiments
- Sensors
- Administrative records
- Digital platforms

- The selected method affects **privacy, accuracy, cost, and participant experience**.
- An ethical method produces useful evidence with the **least reasonable burden and risk**.
- Example: an anonymous survey may be more appropriate than recorded interviews for sensitive workplace experiences.

## B2.5 Ethical Sampling and Participant Inclusion

- **Sampling** determines whose experiences are represented in the dataset.
- Excluding important groups can produce **inaccurate conclusions** and **unequal benefits**.
- Ethical sampling considers:
  - accessibility,
  - diversity,
  - population coverage,
  - barriers to participation.
- Example: an **online-only survey** may underrepresent people with limited internet access.

## B2.6 Informed Consent

- Communicates the project's **purpose, procedures, risks, benefits, and intended data uses**.
- Consent information must be **understandable** and presented **before participation begins**.
- Valid consent requires a **meaningful choice**, not hidden or confusing agreement.
- Must allow **withdrawal**.

Sample statement: *"The survey collects anonymous study habits for academic research, takes approximately ten minutes, and allows withdrawal before submission."*

## B2.7 Voluntary Participation and Coercion

- **Voluntary participation** means refusal does **not** produce unfair punishment or loss of essential services.
- **Power relationships** can create pressure even without a direct threat.
- Special care is needed when data collectors have **authority over participants** (e.g., a professor recruiting their own class).

## B2.8 Deception and Debriefing

- **Deception** = withholding or altering information about a study's true purpose.
- It requires **strong justification** because it limits informed decision-making.
- When ethically approved deception is necessary, **prompt debriefing** explains the actual purpose and provides appropriate options.

## B2.9 Vulnerable Populations

- May have limited ability to **refuse participation** or **protect their interests**.
- Vulnerability can arise from:
  - age,
  - disability,
  - economic dependence,
  - legal status,
  - illness,
  - institutional authority.
- Additional safeguards reduce exploitation and disproportionate harm.

## B2.10 Data from Minors

- Requires **heightened protection** because children may not fully understand long-term privacy consequences.
- Requires **parental/guardian permission** plus **age-appropriate assent** from the minor.
- Collection limited to information **necessary for the activity**.

## B2.11 Sensitive Personal Information

- Includes **health, biometric, financial, religious, political, and sexual** information.
- Unauthorized disclosure may cause:
  - discrimination,
  - embarrassment,
  - identity theft,
  - personal danger.
- Stronger **justification and protection** are required as sensitivity increases.

## B2.12 Observation and Digital Surveillance

- **Observation** can collect information without interrupting natural behavior.
- **Digital surveillance** can continuously record **location, communication, activity, or device information**.
- Ethical use requires:
  - necessity,
  - clear boundaries,
  - notice where appropriate,
  - protection against excessive monitoring.

## B2.13 Ethical Web Scraping

- Public availability does **not** automatically remove:
  - privacy concerns,
  - ownership concerns,
  - contractual concerns,
  - contextual concerns.
- Ethical scraping considers:
  - the site's **terms**,
  - **access rules**,
  - **server load**,
  - **information sensitivity**,
  - **expected use**.

## B2.14 Social Media Data Collection

- Content may be **publicly visible** while still belonging to a **particular social context**.
- Large-scale extraction can expose users to **attention and consequences they did not anticipate**.
- Ethical collection considers:
  - **identifiability**,
  - **quotation**,
  - **consent**,
  - **platform rules**,
  - **potential harm**.
- Example: publishing a user's exact post can let a search engine reveal the person's identity even after the username is removed.

## B2.15 API Access and Platform Terms

- An API provides structured access to a platform's data.
- **Authentication scopes, rate limits, and usage conditions** define how access may be used.
- **Technical access does not automatically provide ethical permission** for every possible analysis.

## B2.16 Third-Party and Public Datasets

- Examine:
  - **license**,
  - **source**,
  - **collection method**,
  - **original purpose**.
- A dataset may contain **personal information** or **historical biases** even when freely downloadable.
- Responsible reuse depends on **compatibility** between original context and the new objective.

**Dataset checklist:** source, license, consent, population, collection date, variables, limitations, permitted uses.

## B2.17 Sensor, Location, and Biometric Data

- Sensors can continuously collect information about **movement, environment, physiology, and behavior**.
- Location and biometric records are **especially sensitive**.
- Safeguards:
  - **limited precision**,
  - **restricted access**,
  - **short retention**,
  - **secure processing**.

## B2.18 Participant Compensation and Incentives

- Compensation recognizes **time, effort, and expenses** associated with participation.
- **Excessive incentives** can improperly influence people to accept risks they would normally reject.
- Ethical compensation is **fair, proportionate, and clearly separated from guaranteed benefits**.

## B2.19 Ethical Data Collection Checklist

**Purpose → Necessity → Consent → Inclusion → Security → Retention → Sharing → Review**

- The plan defines purpose, variables, population, consent process, security controls.
- It evaluates **sensitivity, inclusion, retention, sharing, and secondary uses**.
- **Unresolved high-risk concerns** require revision or **independent ethical review before collection begins**.

---

# MODULE B3: PRIVACY, CONSENT, AND SECURITY

## B3.1 Privacy, Confidentiality, and Anonymity

| Term | Meaning |
|---|---|
| **Privacy** | A person's **control over access** to personal life and information |
| **Confidentiality** | How authorized holders **protect information from unauthorized disclosure** |
| **Anonymity** | An individual's identity is **neither collected nor reasonably connected** to the data |

- Example: an anonymous survey collects no identifiers; a confidential survey stores names separately under restricted access.

## B3.2 Philippine Data Privacy Act of 2012

- **Republic Act No. 10173** establishes protections for personal information processed in **information and communication systems**.
- Recognizes **data subject rights**.
- Assigns responsibilities to organizations that **control or process** personal data.
- Requirements apply **alongside ethical responsibilities** for preventing harm and respecting individuals.
- Example: a university database must protect enrollment information, provide authorized access, and provide procedures for correction.

## B3.3 Transparency, Legitimate Purpose, and Proportionality

| Principle | Meaning |
|---|---|
| **Transparency** | Understandable information about the **nature, purpose, and extent** of processing |
| **Legitimate purpose** | Processing follows a **declared and lawful objective** |
| **Proportionality** | Processing is **adequate, relevant, suitable, necessary, and not excessive** |

- Example: a scholarship form requests family income because it is relevant, but does not request unrelated information.

## B3.4 Lawful Basis for Processing

- Personal data processing requires an appropriate **legal basis**.
- **Consent is one possible basis**, but **not the only basis** for every type of processing.
- The selected basis must match the **purpose, relationship, and circumstances**.
- Other bases include **legal obligation, contract**, or another recognized basis.

## B3.5 Privacy Notices and Clear Communication

- A privacy notice explains:
  - what information is collected,
  - why it is collected,
  - how it will be used,
  - retention,
  - sharing,
  - contact information,
  - applicable data-subject rights.
- **Clear language** is more ethical and meaningful than dense statements designed only to obtain formal acceptance.

**Suggested headings:** Information collected, purpose, recipients, retention period, rights, privacy contact.

## B3.6 Consent Records and Withdrawal

- Consent records provide **evidence** of what information was presented and what choice was made.
- Withdrawal procedures must be **accessible** and explained **before participation**.
- The effect of withdrawal depends on:
  - processing basis,
  - project stage,
  - applicable requirements.
- Record the **notice version, date, selected options, and withdrawal status**.

## B3.7 Data Anonymization

- Removes or transforms identifying information so individuals are **not reasonably identifiable**.
- **Direct identifiers alone are not the only concern** — combinations of **indirect variables** can reveal identity.
- Proper anonymization significantly reduces privacy risk but may also reduce **analytical detail**.

```python
anonymous_data = data.drop(
    columns=["full_name", "email", "student_number"]
)
anonymous_data["age_group"] = pd.cut(
    anonymous_data["age"],
    bins=[0, 17, 24, 34, 49, 100]
)
```

## B3.8 Data Pseudonymization

- Replaces direct identifiers with **codes or artificial identifiers**.
- Information can still be linked to individuals when the **separate identification key** is available.
- Pseudonymized records **remain sensitive** and require **access control**.

```python
import hashlib

def create_code(value):
    return hashlib.sha256(value.encode()).hexdigest()[:12]

data["participant_code"] = data["student_number"].apply(create_code)
data = data.drop(columns=["student_number"])
```

## B3.9 Reidentification Risk

- Occurs when **known anonymous records are connected to individuals**.
- Rare combinations of **age, occupation, location, and event dates** can function like identifiers.
- Risk assessment must consider **other datasets** that could be combined with the released information.
- Example: exact age + small village + rare medical condition may identify a single resident.

## B3.10 Administrative, Physical, and Technical Safeguards

| Safeguard | Examples |
|---|---|
| **Administrative** | Policies, training, authorization, procedures, incident response |
| **Physical** | Protection of devices, offices, records, storage locations |
| **Technical** | Authentication, encryption, access logging, backups, network protection |

- Example: confidentiality training + locked storage + multifactor authentication + encrypted backups.

## B3.11 Access Control and Least Privilege

- **Access control** limits information to **authorized people and approved purposes**.
- **Least privilege** gives only the **minimum permissions** required for a role.
- **Periodic review** removes access that is outdated, unnecessary, or associated with former personnel.
- Example: an analyst can access responses but **not** the separate file containing participant names.

## B3.12 Encryption and Secure Data Transfer

- Encryption converts readable information into **protected data** requiring an authorized **key**.
- **Encryption at rest** protects stored files.
- **Encryption in transit** protects data moving across networks.
- **Secure key management** is necessary because exposed keys defeat the protection.
- Use encrypted institutional platforms, not personal email attachments.

## B3.13 Data Retention and Secure Deletion

- A **retention schedule** defines how long each category of data remains necessary.
- Keeping personal data **indefinitely** increases exposure without improving research value.
- **Secure deletion** removes expired records, temporary exports, backups, and unnecessary copies.
- Example: delete identifying information after verification, while retaining anonymized statistical results for reproducibility.

## B3.14 Data-Sharing Agreements

- Define:
  - data,
  - purpose,
  - recipients,
  - safeguards,
  - responsibilities,
  - permitted uses,
  - retention,
  - return/deletion,
  - breach procedures.
- Restrict **unauthorized redistribution** and **unrelated secondary processing**.

## B3.15 Data Breaches and Incident Response

- **Data breach** = unauthorized **access, disclosure, alteration, loss, or destruction** of protected information.
- **Incident response** includes:
  1. Containment
  2. Assessment
  3. Documentation
  4. Recovery
  5. Required communication
- A prepared response plan reduces confusion and limits further harm.
- Example: a laptop with unencrypted participant records is lost during travel.

## B3.16 Cloud Storage and Cross-Border Processing

- Cloud systems may store/process data in **different jurisdictions**.
- Vendor practices, contracts, security controls, and data locations affect **privacy and regulatory risk**.
- Evaluate:
  - provider encryption,
  - access,
  - data location,
  - subcontractors,
  - breach procedures,
  - backup policy,
  - deletion guarantees.

## B3.17 Privacy Impact Assessment (PIA)

- Identifies how a project **collects, uses, stores, shares, and deletes** personal data.
- Evaluates **threats, affected individuals, possible consequences, and safeguards**.
- Conducted **before implementation** so risky features can be redesigned.

**PIA flow:** Describe processing → Identify risks → Evaluate severity → Select controls → Document decisions → Review changes.

---

# MODULE B4: DATA QUALITY, OWNERSHIP, AND INTEGRITY

## B4.1 Data Quality as an Ethical Responsibility

- Poor-quality data can create **incorrect conclusions** and **harmful decisions**.
- Quality problems become **ethical problems** when they systematically disadvantage individuals or communities.
- **Accuracy** must be considered alongside **privacy, fairness, and security**.
- Example: incorrect addresses in an emergency-assistance database prevent eligible households from receiving support.

## B4.2 Accuracy and Source Verification

| Term | Meaning |
|---|---|
| **Accuracy** | How closely recorded values represent the real conditions being measured |
| **Source verification** | Checks **authenticity, credibility, collection procedures, and possible errors** |

- Important decisions require evidence from sources **appropriate to the intended use**.
- Example: verify unofficial social-media employment statistics against an authoritative government dataset.

## B4.3 Completeness and Nonresponse

- **Completeness** = whether required **records and variables** are sufficiently available.
- **Nonresponse** creates systematic gaps when certain groups are **less likely to participate**.
- Reporting only the number of completed responses can **hide important coverage limitations**.

```python
missing_report = (
    data.isna()
    .mean()
    .mul(100)
    .sort_values(ascending=False)
)
print(missing_report)
```

## B4.4 Consistency and Validity

| Term | Meaning |
|---|---|
| **Consistency** | Values follow compatible **definitions, units, and formats** across records |
| **Validity** | Values satisfy expected **types, ranges, and domain rules** |

- Automated validation detects **obvious problems**; domain knowledge explains whether **unusual values are possible**.

```python
valid_age = data["age"].between(15, 100)
valid_score = data["score"].between(0, 100)
invalid_records = data[~(valid_age & valid_score)]
```

## B4.5 Data Provenance and Lineage

- **Provenance** records **where information originated and how it was collected**.
- **Lineage** tracks **transformations** from original source to final dataset/report.
- These records support **accountability, reproducibility, and error investigation**.

**Example lineage:** Survey form → Raw CSV → Cleaned table → Analytical dataset → Dashboard

## B4.6 Metadata and Documentation

- **Metadata** describes variables, units, formats, sources, collection dates, and conditions.
- A **data dictionary** prevents inconsistent interpretation among analysts and future users.
- Documentation preserves **limitations and ethical restrictions** not visible in the values.
- Example: `income_monthly` = reported household income in Philippine pesos, optional, collected in ranges.

## B4.7 Ownership, Stewardship, and Licensing

| Term | Meaning |
|---|---|
| **Ownership** | Legal rights and control associated with a dataset |
| **Stewardship** | Responsibility to **protect, maintain, and use** data appropriately |
| **Licensing** | Conditions under which data may be **copied, modified, shared, or published** |

- Access does **not** equal ownership or authority to publish.

## B4.8 Open Data and Responsible Reuse

- **Open data** supports transparency, research, innovation, and public participation.
- Open availability does **not** eliminate requirements involving:
  - attribution,
  - licensing,
  - privacy,
  - contextual interpretation.
- Responsible reuse confirms the data remain **suitable for the new question**.

## B4.9 Ethical Treatment of Missing Values

- Missing values may reflect:
  - refusal,
  - technical failure,
  - inapplicability,
  - systematic exclusion.
- Deleting or filling these values can **change relationships and group comparisons**.
- The selected treatment must be **justified and documented**.

```python
median_income = data["income"].median()
data["income_missing"] = data["income"].isna()
data["income"] = data["income"].fillna(median_income)
```

## B4.10 Ethical Treatment of Duplicates and Outliers

- **Duplicates** can exaggerate the influence of particular observations.
- **Outliers** may represent errors, rare events, or important minority experiences.
- Removing values based only on **inconvenience** can distort findings and hide meaningful variation.

```python
duplicates = data[data.duplicated(subset=["record_id"], keep=False)]
possible_outliers = data[
    data["transaction"] > data["transaction"].quantile(0.99)
]
```

## B4.11 Fabrication, Falsification, and Manipulation

| Misconduct | Definition |
|---|---|
| **Fabrication** | Creates observations or results that **never existed** |
| **Falsification** | Changes data, procedures, or outcomes to produce a **desired conclusion** |
| **Manipulation** | Includes selective transformations designed to **hide contradictory evidence** |

- Example: replacing unfavorable survey responses with invented positive values before presenting the report.

## B4.12 Audit Trails and Version Control

- An **audit trail** records changes to **data, code, parameters, and analytical decisions**.
- **Version control** preserves earlier states and identifies **who introduced each modification**.
- These practices make errors easier to investigate and results easier to reproduce.
- Example: raw data remain **read-only**, while each cleaning step is stored in a **dated script** and change log.

---

# MODULE B5: ETHICS IN DATA ANALYSIS

## B5.1 Ethical Research Questions

- Ethical analysis begins with a **responsible, clearly defined research question**.
- Some questions can **stigmatize** or encourage **intrusive collection** even before analysis begins.
- The expected value of an answer must justify the **required data and possible consequences**.
- Example: predicting which applicants may leave university early could support assistance **or** create unfair labeling.

## B5.2 Analytical Plans and Decision Criteria

- An analytical plan identifies, **before results are known**:
  - variables,
  - methods,
  - comparisons,
  - exclusions,
  - evaluation measures.
- **Predetermined criteria** reduce the temptation to choose only procedures that support a preferred conclusion.
- Changes remain possible but require **explanation and documentation**.
- Example plan: outcome variable, predictor variables, missing-data method, checks, accuracy metric, decision threshold.

## B5.3 Preregistration and Analytical Transparency

- **Preregistration** records **research questions, hypotheses, and planned methods** before examining final results.
- It distinguishes **confirmatory analysis** from later **exploratory discoveries**.
- Transparent reporting of deviations allows useful changes **without concealing** how the analysis evolved.
- Example: an analysis plan is **time-stamped** before the final dataset is released.

## B5.4 Representativeness and Generalizability

| Term | Meaning |
|---|---|
| **Representativeness** | How well a sample reflects the population relevant to the question |
| **Generalizability** | Whether findings can reasonably apply **beyond the observed records** |

- A large sample size **cannot correct systematic exclusion** of important groups.
- Example: a nationwide opinion claim based entirely on one university's online community.

## B5.5 Descriptive vs Inferential Claims

| Type | Meaning |
|---|---|
| **Descriptive** | Summarizes the records **actually observed** |
| **Inferential** | Uses a sample to **estimate or test claims about a larger population** |

- Ethical reporting keeps conclusions **within the level supported by the design and evidence**.
- Example: "Sixty percent of respondents agreed" is descriptive, not a claim about the whole population.

## B5.6 Correlation vs Causation

- **Correlation** = variables **change together** within observed data.
- **Causation** = changing one factor **produces a change** in another.
- Causal language requires an **appropriate design** and careful treatment of **alternative explanations**.
- Example: higher social-media use is **associated with** lower grades, but that does **not prove** social media caused the lower grades.

## B5.7 Confounding Variables

- A **confounder** is related to **both** the explanatory variable and the outcome.
- It can create **or hide** an apparent relationship.
- Ethical interpretation considers credible confounders **before recommending action**.
- Example: work hours may influence both class attendance and academic performance.

## B5.8 Data Dredging and P-Hacking

| Term | Meaning |
|---|---|
| **Data dredging** | Searching many relationships until an interesting result appears |
| **P-hacking** | Repeatedly changing tests, variables, exclusions, or stopping rules to obtain statistical significance |

- These increase the likelihood of presenting **random patterns as meaningful findings**.
- Example: twenty outcomes tested, but only the single significant result reported.

## B5.9 Multiple Comparisons and False Discoveries

- Each statistical test carries some possibility of a **false positive**.
- Many comparisons increase the chance that **at least one appears significant by accident**.
- Appropriate **corrections** and **complete reporting** reduce misleading discoveries.

```python
from statsmodels.stats.multitest import multipletests

adjusted = multipletests(p_values, method="fdr_bh")
rejected = adjusted[0]
adjusted_p_values = adjusted[1]
```

## B5.10 Selective Reporting and Cherry-Picking

- **Selective reporting** presents evidence that supports a preferred claim while hiding conflicting relevant results.
- **Cherry-picking** may involve observations, variables, subgroups, time periods, or analytical methods.
- Complete reporting allows the audience to evaluate the **full strength of the evidence**.
- Example: advertising the month with the greatest sales increase while omitting a year-long decline.

## B5.11 Consequences of Missing-Data Treatment

- Different missing-data methods can produce **different estimates and conclusions**.
- **Complete-case analysis** may disproportionately remove records from groups with higher nonresponse.
- **Sensitivity analysis** compares reasonable methods to see whether findings remain stable.

```python
mean_result = data["income"].fillna(data["income"].mean()).mean()
median_result = data["income"].fillna(data["income"].median()).mean()
print(mean_result, median_result)
```

## B5.12 Consequences of Outlier Treatment

- Removing an extreme observation may **reduce error** or **erase a valid experience**.
- Outlier decisions require investigation of:
  - source,
  - plausibility,
  - influence,
  - analytical purpose.
- Report results **with and without** influential values when uncertainty remains.

## B5.13 Ethical Variable Selection

- Variable selection affects **model performance, interpretation, and fairness**.
- A variable may be **statistically useful** while being **intrusive, unstable, or unrelated** to a legitimate objective.
- Selection requires consideration of **necessity and possible social consequences**.
- Example: browsing history slightly improves a hiring model but creates substantial privacy and relevance concerns.

## B5.14 Proxy Variables and Hidden Discrimination

- A **proxy variable** indirectly represents a **protected or sensitive characteristic**.
- Examples: school attended, language, purchasing patterns, residential area.
- Removing an **explicit protected attribute** does **not guarantee** a model is free from discrimination.

## B5.15 Model Complexity and Explainability

- Complex models can capture relationships simpler methods miss.
- Greater complexity can make **predictions and failures more difficult to explain**.
- High-impact decisions require balance among **performance, interpretability, fairness, and oversight**.
- Example: a transparent scoring model may be preferable when rejected applicants have a right to understand the decision.

## B5.16 Training and Testing Separation

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
)
```

- **Training data** estimate model patterns.
- **Testing data** provide an **independent evaluation**.
- Allowing testing information to influence training creates **data leakage** and unrealistic performance results.
- Ethical reporting uses evaluation procedures that reflect **actual future use**.

## B5.17 Reporting Uncertainty and Error

- Estimates contain uncertainty from **sampling, measurement, modeling, and changing conditions**.
- A single number can create **false confidence** when its possible error is hidden.
- Report **intervals, error measures, and plain-language qualifications**.
- Example: report an estimated 58% support **with its confidence interval**, not 58% as an exact population value.

## B5.18 Limitations, Reproducibility, and Replication

| Term | Meaning |
|---|---|
| **Limitations** | Conditions that restrict interpretation or application of results |
| **Reproducibility** | Same data and procedures generate the reported findings |
| **Replication** | Whether a finding appears again using **new data** or an **independent study** |

- Include code, software versions, data dictionary, assumptions, and known limitations.

---

# MODULE B6: BIAS AND FAIRNESS

## B6.1 Meaning of Bias

- **Bias** is a **systematic tendency** that moves data, analysis, decisions, or conclusions **away from an appropriate standard**.
- It can enter through:
  1. collection,
  2. measurement,
  3. labeling,
  4. modeling,
  5. interpretation,
  6. deployment.
- Bias does **not always involve intentional prejudice**.
- Example: a voice-recognition model performs poorly for accents uncommon in its training data.

## B6.2 Types of Bias

| Type | Definition | Example |
|---|---|---|
| **Historical bias** | Data reflect past discrimination or unequal access; the model learns and continues those patterns | Recruitment model trained on historically unequal hiring records |
| **Selection bias** | Included observations differ systematically from the intended population (convenience sampling, voluntary response, nonresponse, unavailable records) | Satisfaction measured only among customers who posted online reviews |
| **Measurement bias** | A variable is recorded differently across individuals/groups (unsuitable instruments, inconsistent definitions, language differences, unequal observation) | Online exam measures internet stability as well as subject knowledge |
| **Label / annotation bias** | Human judgments, institutional practices, or unclear instructions introduce bias into labels | Reviewers label comments inconsistently |
| **Representation bias** | Some populations have insufficient examples in the dataset; overall accuracy hides poor subgroup performance | Underrepresented groups in training data |
| **Aggregation bias** | One model or conclusion is applied to groups with meaningfully different patterns; averages hide variation | One medical risk threshold used across age groups with different baseline risks |
| **Proxy discrimination** | Apparently neutral variables reproduce decisions associated with protected characteristics | Occupation history closely reflects gender inequality |

- **Accurate reproduction of historical decisions is not automatically fair.**

## B6.3 Definitions and Measures of Fairness

Fairness can refer to:
- **equal selection rates**,
- **equal opportunity**,
- **balanced error rates**,
- **consistency**.

- Different definitions emphasize **different harms** and may produce **conflicting evaluations**.
- The appropriate measure depends on:
  - context,
  - stakeholder interests,
  - consequences of errors.
- Example measures: selection rate, true-positive rate, false-positive rate, error rate **by group**.

## B6.4 Fairness Trade-Offs

- Improving one fairness measure can **reduce predictive performance**.
- These trade-offs **cannot be resolved through mathematics alone** because they involve **values and consequences**.
- Decisions require **transparent justification and stakeholder participation**.

## B6.5 Subgroup Performance Evaluation

- **Aggregate performance can conceal unequal model behavior** across demographic or contextual groups.
- Subgroup evaluation compares **accuracy, sensitivity, specificity, and error patterns**.
- Very small subgroups require careful interpretation because estimates may be **unstable**.

```python
from sklearn.metrics import accuracy_score

for group, subset in results.groupby("group"):
    score = accuracy_score(subset["actual"], subset["predicted"])
    print(group, round(score, 3))
```

## B6.6 Conducting a Fairness Audit

**Define purpose → Inspect data and groups → Compare metrics → Investigate causes → Apply safeguards → Monitor outcomes**

- A fairness audit examines:
  - project purpose,
  - data representation,
  - variables,
  - model performance,
  - real-world outcomes.
- It documents affected groups and burdens, and investigates where **unequal errors** arise.
- Auditing **continues after deployment** because populations and conditions change.

## B6.7 Human Oversight, Explanation, and Appeals

- **Human oversight** reviews unusual, uncertain, or high-impact automated decisions.
- Meaningful oversight requires **authority, information, time, and freedom to challenge the system**.
- **Explanation and appeal procedures** allow affected individuals to question and correct decisions.
- Example: an automated scholarship rejection receives independent review after the applicant identifies an incorrect income record.

---

# MODULE B7: ETHICS IN DATA VISUALIZATION

## B7.1 Ethical Visual Encoding

- Visual encoding represents data through:
  - **position**,
  - **length**,
  - **shape**,
  - **size**,
  - **color**,
  - **area**.
- The chosen encoding affects how accurately the audience perceives differences.
- Ethical design avoids emphasis that **exaggerates or minimizes** underlying values.
- **Position and length** support more precise comparisons than decorative area or volume.

## B7.2 Selecting an Appropriate Chart Type

| Data Structure / Purpose | Chart |
|---|---|
| Category comparison | **Bar chart** |
| Ordered trend over time | **Line chart** |
| Relationship between variables | **Scatterplot** |
| Distribution | **Histogram** |

- An unsuitable chart can confuse the audience or suggest a relationship that does not exist.

## B7.3 Axes, Baselines, and Truncated Scales

- Axis limits control the **apparent magnitude** of differences.
- **Bar charts generally require a zero baseline** because bar length represents value directly.
- Truncated axes may be acceptable in some cases but must **not be used to exaggerate**.
- Example: an increase from 95 to 97 looks enormous when the vertical axis starts at 94.

## B7.4 Logarithmic Scales and Transformations

- A **logarithmic scale** represents **multiplicative change** and compresses large value ranges.
- It can reveal meaningful patterns across several **orders of magnitude**.
- Clear labels and explanations are required; otherwise viewers may misread the spacing as linear.

## B7.5 Time-Period Selection

- The selected time period can change the **apparent direction and strength** of a trend.
- Starting at an unusual point can create a misleading impression.
- Ethical reporting uses a **relevant period** and explains any restricted date range.
- Example: starting a market report immediately after a temporary collapse to create the appearance of continuous growth.

## B7.6 Denominators, Percentages, and Context

- Percentages require a **clearly defined denominator**.
- Relative changes can appear dramatic when the original number is **small**.
- Ethical communication presents relevant **counts, rates, comparison groups, and population size**.
- Example: "Cases doubled" is clearer as "cases increased from 2 to 4 among 20,000 observations."

## B7.7 Ethical Use of Color

- Color directs attention, organizes categories, and communicates **intensity or direction**.
- Inappropriate color choices can create **emotional bias** or imply order among unrelated groups.
- Palettes must match the meaning of the data:
  - **Sequential** — ordered magnitude
  - **Diverging** — values around a meaningful midpoint
  - **Categorical** — unrelated categories

## B7.8 Accessible Visualization Design

- Accessibility considers:
  - color-vision differences,
  - low vision,
  - screen readers,
  - varying levels of data literacy.
- Important distinctions must **not rely on color alone**.
- Use **labels, sufficient contrast, patterns, descriptions, and readable text**.
- Example: a line chart uses color, line style, and direct labels together.

## B7.9 Problems with 3D Charts, Area, and Volume

- Three-dimensional effects can distort **lengths, angles, areas, and relative positions**.
- Viewers often **underestimate or overestimate** values represented by area and volume.
- **Simple two-dimensional displays** usually support more accurate comparisons.
- Example: a 3D pie chart makes slices appear larger or smaller depending on position.

## B7.10 Displaying Uncertainty and Error

- Uncertainty displays show the range of values reasonably compatible with an estimate.
- Use:
  - **error bars**,
  - **confidence intervals**,
  - **probability distributions**,
  - **scenario bands**.
- These prevent **false precision**; the type and meaning of uncertainty must be clearly explained.

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.pointplot(
    data=data,
    x="group",
    y="score",
    errorbar=("ci", 95)
)
plt.ylabel("Mean score with 95% confidence interval")
plt.show()
```

## B7.11 Ethics in Maps and Geographic Visualizations

- Maps can **exaggerate large geographic areas** even when those areas contain few people.
- **Choropleth maps** usually require **normalized rates**, not raw population counts.
- Interpretation is also influenced by:
  - projection,
  - boundaries,
  - missing regions,
  - geographic aggregation.
- Example: mapping total disease cases by province may display **population size** rather than **disease risk**.

## B7.12 Ethical Dashboard Design

- Dashboards combine indicators, filters, comparisons, and visual summaries for repeated decision-making.
- **Default settings and highlighted metrics** influence which issues receive attention.
- Ethical dashboards:
  - preserve context,
  - reveal definitions,
  - avoid concealing unfavorable information.
- Example: show both total outcomes **and** subgroup differences, not only the overall average.

## B7.13 Annotations, Sources, and Disclosure

- **Annotations** explain important events, definitions, changes, and unusual patterns.
- **Source information** lets the audience assess credibility and context.
- **Disclosures** identify transformations, exclusions, uncertainty, and limitations.

**Recommended footer:** source, collection date, update date, population, units, exclusions, analytical notes.

---
---

# DRILL A — MODIFIED MULTIPLE CHOICE

## Part A (Web Scraping)

1. Web scraping is best defined as:
   a) Manually copying webpage text
   b) The automated retrieval of selected information from webpages
   c) Encrypting website traffic
   d) Building a website database

2. Which method systematically follows links across connected pages?
   a) API extraction
   b) Web scraping
   c) Web crawling
   d) HTML parsing

3. Which tool is recommended for multiple connected and paginated pages?
   a) Beautiful Soup
   b) Scrapy
   c) Excel
   d) Regular expressions only

4. Which is NOT a component of a URL?
   a) Scheme
   b) Domain
   c) Query parameters
   d) DOM

5. HTTP status 429 means:
   a) Successful request
   b) Access forbidden
   c) Too many requests
   d) Server error

6. Which one is the correct pipeline order?
   a) Data Retrieval → Research Question → Storage → Parsing
   b) Research Question → Source Assessment → Permission Check → Data Retrieval → Parsing → Cleaning → Validation → Storage → Documentation
   c) Permission Check → Documentation → Cleaning → Research Question
   d) Parsing → Research Question → Retrieval → Validation

7. Public visibility of a webpage means:
   a) Automated collection is always allowed
   b) Automated collection is never allowed
   c) Automated collection may still be restricted
   d) robots.txt no longer applies

8. `robots.txt` communicates:
   a) The website's database password
   b) Instructions to automated crawlers
   c) The page's HTML version
   d) API keys

9. Data minimization means:
   a) Deleting all data after collection
   b) Collecting only information required by the research question
   c) Collecting every available field
   d) Encrypting all data

10. Which statement about `raise_for_status()` is TRUE?
    a) It parses HTML
    b) It raises an exception for unsuccessful HTTP responses
    c) It joins relative URLs
    d) It creates a DataFrame

11. `find_all()` returns:
    a) The first matching element
    b) Every matching element as a collection
    c) Only attributes
    d) A DataFrame

12. Which Beautiful Soup method returns the first element matching a CSS selector?
    a) `select()`
    b) `select_one()`
    c) `find_all()`
    d) `prettify()`

13. What does `urljoin()` do?
    a) Downloads an image
    b) Combines a base URL and relative path into a complete URL
    c) Validates JSON
    d) Cleans HTML entities

14. In Scrapy, `yield` is used to:
    a) End the entire crawl
    b) Return an extracted record without ending the crawl
    c) Start a virtual environment
    d) Delete duplicates

15. Which Scrapy selector extracts text?
    a) `::attr()`
    b) `::text`
    c) `.getall()`
    d) `response.follow()`

16. Which statement(s) is/are TRUE?
    A. `-O` replaces an existing Scrapy output file.
    B. `response.follow()` can resolve relative paths.
    C. XPath can use predicates.
    a) A only
    b) A and B
    c) B and C
    d) A, B, and C

17. An API key identifies the:
    a) Data subject
    b) Application making the request
    c) Webpage's CSS class
    d) Database schema

18. API pagination is commonly continued using a:
    a) Cookie
    b) Page token
    c) CSS selector
    d) robots.txt rule

19. Which is the safest way to store an API key?
    a) Hard-code it in the script
    b) Commit it to the repository
    c) Store it in an environment variable
    d) Include it in screenshots

20. Which is the correct preferred order?
    a) HTML scraping → API → institutional dataset
    b) Official API → authorized institutional dataset → instructor-provided JSON → HTML scraping only when explicitly permitted
    c) Instructor JSON → HTML scraping → API
    d) Any method, as long as it works

## Part B (Ethics)

21. Data ethics is primarily about:
    a) Technical accuracy only
    b) Moral responsibilities in collecting, processing, analyzing, and communicating data
    c) Avoiding all data collection
    d) Obeying laws only

22. A legally permitted action is:
    a) Always ethical
    b) Never ethical
    c) Possibly unethical if it violates reasonable expectations or creates unnecessary risk
    d) Outside the scope of ethics

23. Which is NOT a core ethical value from the slides?
    a) Honesty
    b) Fairness
    c) Profit maximization
    d) Accountability

24. Purpose limitation means:
    a) Data may be reused for any purpose
    b) Information collected for one purpose is not automatically reused for an unrelated purpose
    c) All data must be deleted immediately
    d) Only government data is regulated

25. Necessity asks:
    a) Whether the data are expensive
    b) Whether the objective can be achieved without collecting the information
    c) Whether the data are numerical
    d) Whether the chart is colorful

26. Which is an example of a vulnerable population?
    a) Adult volunteers with full capacity
    b) Children in an institutional setting
    c) Anonymous online users
    d) Government regulators

27. Deception in research:
    a) Is always acceptable
    b) Requires strong justification and prompt debriefing when approved
    c) Never requires review
    d) Is the same as informed consent

28. Which is the correct definition of anonymity?
    a) Names are stored separately
    b) Identity is neither collected nor reasonably connected to the data
    c) Data are encrypted
    d) Data are public

29. RA No. 10173 is also known as:
    a) The Philippine Data Privacy Act of 2012
    b) The Clean Air Act
    c) The Intellectual Property Code
    d) The Anti-Cybercrime Law

30. Pseudonymization:
    a) Guarantees that reidentification is impossible
    b) Replaces identifiers with codes but may allow linkage via a separate key
    c) Deletes all data
    d) Is the same as aggregation

31. Least privilege means:
    a) Giving every employee admin access
    b) Giving only the minimum permissions required for a role
    c) Sharing passwords
    d) Disabling encryption

32. Which is a technical safeguard?
    a) Policies and training
    b) Locked offices
    c) Encryption and access logging
    d) Staff authorization procedures

33. Encryption at rest protects:
    a) Data moving across networks
    b) Stored files
    c) Only printed records
    d) Only API keys

34. Which is a correct incident-response sequence?
    a) Communication → Ignore → Delete → Containment
    b) Containment → Assessment → Documentation → Recovery → Required communication
    c) Recovery → Containment → Fabrication
    d) Documentation → Delete all evidence

35. Which is the correct PIA flow?
    a) Describe processing → Identify risks → Evaluate severity → Select controls → Document decisions → Review changes
    b) Collect data → Publish → Delete
    c) Identify risks → Ignore risks → Deploy
    d) Select controls → Describe processing → Delete

36. Which is the best definition of validity?
    a) Values follow compatible formats
    b) Values satisfy expected types, ranges, and domain rules
    c) Data are complete
    d) Data are encrypted

37. Data provenance records:
    a) Only the final chart
    b) Where, when, and how information originated and was collected
    c) Only the number of rows
    d) The analyst's opinion

38. Fabrication means:
    a) Changing procedures to get a desired result
    b) Creating observations or results that never existed
    c) Removing duplicates
    d) Reporting uncertainty

39. A confounder is related to:
    a) Only the outcome
    b) Both the explanatory variable and the outcome
    c) Neither variable
    d) Only the sample size

40. P-hacking involves:
    a) Reporting all results transparently
    b) Repeatedly changing tests, variables, exclusions, or stopping rules to obtain significance
    c) Using a zero baseline
    d) Preregistering a plan

41. Which is an example of selection bias?
    a) Measuring satisfaction only among customers who posted online reviews
    b) Using a validated instrument
    c) Random sampling
    d) Reporting confidence intervals

42. Proxy discrimination occurs when:
    a) A protected attribute is removed and discrimination disappears
    b) Apparently neutral variables reproduce decisions associated with protected characteristics
    c) The model has high accuracy
    d) The dataset is large

43. Which fairness measure is mentioned in the slides?
    a) Equal selection rates
    b) Equal file sizes
    c) Equal number of charts
    d) Equal API quotas

44. A fairness audit should continue:
    a) Only before deployment
    b) After deployment, because populations and conditions change
    c) Never
    d) Only if a complaint is filed

45. Bar charts generally require:
    a) A logarithmic axis
    b) A zero baseline
    c) A 3D effect
    d) A truncated axis

46. A choropleth map usually requires:
    a) Raw counts
    b) Normalized rates
    c) 3D volume
    d) A pie chart

47. Which statement(s) is/are TRUE?
    A. Position and length support precise comparisons.
    B. 3D charts improve accuracy.
    C. Color alone is sufficient for accessibility.
    a) A only
    b) A and B
    c) B and C
    d) All of the above

48. Error bars and confidence intervals help prevent:
    a) Data collection
    b) False precision
    c) Informed consent
    d) Encryption

---

# DRILL B — MODIFIED TRUE OR FALSE
*If FALSE, correct the statement.*

## Part A (Web Scraping)

1. Web scraping and web crawling are exactly the same thing.
2. An official API is generally preferred when a platform provides one.
3. `robots.txt` is the only rule a scraper must follow.
4. Publicly visible content may still be protected by copyright.
5. Data provenance records where, when, and how a record was collected.
6. A virtual environment isolates project packages and prevents version conflicts.
7. `timeout` prevents a request from waiting indefinitely.
8. `raise_for_status()` should be ignored to keep the program running.
9. Beautiful Soup converts HTML into a searchable Python object.
10. `find()` returns every matching element.
11. Calling a method on a missing element can crash a program.
12. Relative links can be converted into complete URLs with `urljoin()`.
13. A multi-page scraper should have no page limit.
14. Scrapy is only an HTML-parsing library.
15. `response.follow()` can resolve relative paths.
16. `::text` extracts textual content in Scrapy.
17. The `-O` option in Scrapy replaces an existing output file.
18. An API key identifies the user's password.
19. OAuth authorizes access under defined user permissions.
20. API quotas limit how many requests or cost units an application may consume.
21. Pseudonymization guarantees that reidentification is impossible.
22. A data dictionary should be prepared before collection.
23. Duplicates should be removed without defining comparison fields.
24. Raw data should be overwritten during cleaning.
25. Access denied (403) is a technical obstacle that should be bypassed.

## Part B (Ethics)

26. Data ethics is only about technical accuracy.
27. A legally permitted practice can still be unethical.
28. Transparency requires exposing all confidential information.
29. Accountability disappears when a decision is automated.
30. Ethics applies only after a project is completed.
31. Purpose limitation allows automatic reuse of data for unrelated purposes.
32. Data minimization reduces privacy exposure and possible misuse.
33. Informed consent must be understandable and given before participation.
34. A professor recruiting their own students raises no coercion concerns.
35. Deception never requires justification.
36. Minors require heightened protection and age-appropriate assent.
37. Sensitive personal information includes health, biometric, financial, religious, political, and sexual information.
38. Public social-media content raises no ethical concerns.
39. Technical API access automatically grants ethical permission for every analysis.
40. Anonymization removes or transforms identifying information.
41. Pseudonymized records are no longer sensitive.
42. Least privilege gives only the minimum permissions required for a role.
43. Encryption in transit protects stored files.
44. Keeping personal data indefinitely improves research value.
45. A data breach only includes accidental deletion.
46. A PIA should be conducted before implementation.
47. Poor data quality can become an ethical problem when it systematically disadvantages groups.
48. A large sample size can correct systematic exclusion of important groups.
49. Correlation proves causation.
50. Preregistration distinguishes confirmatory analysis from later exploratory discoveries.
51. Data dredging increases the chance of presenting random patterns as meaningful findings.
52. Complete-case analysis can disproportionately remove groups with higher nonresponse.
53. Removing an explicit protected attribute guarantees a model is fair.
54. A fairness trade-off can be resolved by mathematics alone.
55. A zero baseline is generally required for bar charts.
56. A truncated axis may be used to exaggerate differences.
57. 3D charts usually improve the accuracy of comparisons.
58. A choropleth map of raw counts may mostly display population size.
59. Dashboards should hide unfavorable information to avoid confusion.
60. Important visual distinctions should not rely on color alone.

---

# DRILL C — ENUMERATION / EXEMPLIFICATION

## Part A (Web Scraping)

1. Differentiate **web scraping, web crawling, and API extraction**.
2. Give the **three tool-selection situations** and the appropriate method for each.
3. List the **nine stages** of the general extraction pipeline.
4. Identify the **five URL components** and give one example of each.
5. Give the **five HTTP status codes** and their meanings.
6. Exemplify **static vs dynamic pages** and the appropriate method for each.
7. List the **eight pre-collection checklist items**.
8. Give **four responsible request-frequency practices**.
9. List **three data-provenance fields**.
10. Name the **five libraries** used and their purposes.
11. List **five components of a Scrapy project** and their purposes.
12. Give **five Scrapy settings** and what each controls.
13. Give the **API-first preferred order** (four levels).
14. List the **four public/private classifications** and the required handling for each.
15. List **five fields** that may be included in a social-media data dictionary.
16. Give **three dataset inspection checks** after creating a DataFrame.
17. List **five common extraction problems** and the appropriate response.
18. Give **three examples of web-extraction applications**.
19. List **three safeguards for sensor/location/biometric data** (from the ethics deck).
20. List **four formats/tools** for exporting collected data.

## Part B (Ethics)

1. List the **six core ethical values** in data science.
2. Give **five possible harms** of a data project.
3. List the **six stakeholder groups**.
4. Give the **four life-cycle stages** and an ethical concern for each.
5. Define **purpose limitation, necessity, proportionality, and data minimization**.
6. List **seven data-collection methods**.
7. List **five elements of informed consent**.
8. Give **six sources of vulnerability**.
9. List **five categories of sensitive personal information**.
10. Give the **eight-step ethical data-collection checklist**.
11. Differentiate **privacy, confidentiality, and anonymity**.
12. List the **three safeguard categories** with two examples each.
13. Give the **five incident-response steps**.
14. List **seven cloud-evaluation criteria**.
15. Give the **six-step PIA flow**.
16. Differentiate **accuracy, completeness, consistency, and validity**.
17. Differentiate **provenance and lineage**, and give the example lineage chain.
18. Differentiate **ownership, stewardship, and licensing**.
19. Give **three research-misconduct terms** and their definitions.
20. List **six places bias can enter** a data project.
21. Give **seven types of bias** with one example each.
22. List **four fairness definitions/measures**.
23. Give the **six-step fairness-audit flow**.
24. List **five ethical visual-encoding variables**.
25. Give the **chart-type mapping** for category, time, relationship, and distribution.
26. List **four uncertainty-display methods**.
27. Give the **recommended visualization footer items**.

---

# DRILL D — ESSAY MODEL ANSWERS

## Part A (Web Scraping)

**1. Explain web scraping and why it matters.**
Web scraping is the automated process of retrieving selected information from webpages. A program requests a page, examines its HTML structure, identifies the elements containing the required data, and converts the extracted information into structured records. It matters because websites hold large amounts of useful information for research and decision-making, while manual copying is slow and inconsistent. Automated extraction makes collection efficient and reproducible. However, the quality of the final analysis still depends on whether the data are relevant, complete, accurate, and ethically obtained.

**2. Compare web scraping, web crawling, and API extraction.**
Web scraping extracts selected information from the content of a webpage. Web crawling systematically follows links to discover and process multiple connected pages. API extraction requests structured information directly from an authorized application interface. The appropriate method depends on the source: Beautiful Soup for small or moderately sized static pages, Scrapy for multiple connected and paginated pages, and an official API for structured platform or social-media information. An API-first approach is preferred because it is structured, authorized, and governed by defined fields and limits.

**3. Explain the general extraction pipeline.**
A responsible pipeline begins with a clear research question, because the question determines the required variables. Next is source assessment and a permission check to confirm that the source is appropriate and authorized. Data retrieval follows, then parsing, cleaning, validation, and structured storage. The process concludes with documentation of the source, method, limitations, and ethical decisions. Each stage matters because poor decisions early in the pipeline create risks and errors later.

**4. Explain static vs dynamic pages and how to choose a method.**
A static page sends its main content inside the original HTML response, so Beautiful Soup or Scrapy can extract the data directly. A dynamic page uses JavaScript to retrieve or generate content after the initial load, so some information visible in the browser may be absent from the original HTML. Before collecting, the researcher should inspect the page source and Fetch/XHR requests. If the data appear only after JavaScript runs, an official API or authorized JSON endpoint is the appropriate method; HTML scraping is used only when explicitly permitted.

**5. Explain responsible request frequency and robots.txt.**
Sending too many requests in a short period can overload a server or activate protective controls. A responsible scraper limits the number of pages, inserts delays, and observes server responses; it slows down or stops when the server reports excessive activity such as HTTP 429. A website may also publish `robots.txt` to communicate which paths crawlers are allowed or discouraged from visiting. These instructions must be considered together with platform terms, privacy requirements, and explicit authorization. robots.txt does not by itself grant ethical or legal permission.

**6. Explain the Beautiful Soup extraction workflow.**
The workflow starts with `requests.get()` and a timeout, followed by `raise_for_status()` inside a try/except block. The HTML text is converted into a searchable object with `BeautifulSoup(response.text, "html.parser")`. The researcher inspects the parsed document with `prettify()` and locates elements using `find()`, `find_all()`, `select_one()`, or `select()`. Text is extracted with `get_text(strip=True)` and attributes with `get()` or dictionary indexing. Repeated records are collected by iterating over shared containers and storing consistent field names, while defensive checks prevent crashes when elements are missing.

**7. Compare Beautiful Soup and Scrapy.**
Beautiful Soup is primarily an HTML-parsing library commonly combined with `requests`. It is excellent for small static pages, but multi-page crawling, request scheduling, pipelines, and logging must be handled manually or built custom. Scrapy is a crawling framework that manages requests, responses, callbacks, extraction, and item processing. It provides built-in multi-page crawling, request scheduling, pipelines, logging, statistics, and structured exports. Both can use CSS selectors, but Scrapy provides more project infrastructure. Tool selection depends on source structure, scale, technical complexity, and reproducibility requirements.

**8. Explain the Scrapy architecture and a paginated spider.**
A Scrapy spider generates requests; the downloader fetches responses; the parser/callback extracts items; and item pipelines clean, validate, or store them. Settings control delays, retries, concurrency, logging, and crawl limits. In a paginated spider, the `parse()` method extracts records from the current page, then locates the next-page link and yields `response.follow(next_page, callback=self.parse)`. The crawl ends naturally when the next-page element is absent, and a page limit prevents uncontrolled collection.

**9. Explain API-first social-media extraction.**
Social-media interfaces are dynamic, restricted, and protected by platform controls, so an official API is the preferred method. It provides a structured and accountable way to request approved fields, and it defines authentication, request parameters, and usage limits. The preferred order is official API, authorized institutional dataset, instructor-provided JSON, and HTML scraping only when explicitly permitted. API keys identify the application while OAuth authorizes access under defined user permissions; credentials must be stored securely and never exposed. Requests should use only necessary fields, respect quotas, and stop when limits are reached.

**10. Explain data validation and preservation after extraction.**
After extraction, the dataset should be checked against a schema of expected fields and types. Missing values, duplicates, inconsistent text, invalid dates, and encoding artifacts must be inspected and treated deliberately. Every treatment decision should be documented. Raw data, processed data, logs, and collection metadata should be stored separately so the transformation process can be reviewed and reproduced. The dataset is analysis-ready only after schema, uniqueness, completeness, and value checks are satisfied.

## Part B (Ethics)

**1. Explain why ethics matters in data science.**
Data science influences decisions in education, employment, healthcare, public services, and financial opportunities. Poorly designed practices can expose private information, reinforce discrimination, or produce misleading conclusions. Ethical practice ensures that data-driven decisions respect people while delivering meaningful benefits. A credit-scoring model, for example, may speed up loan processing but unfairly disadvantage groups underrepresented in its training data. Therefore, technical accuracy and legal compliance alone are not enough; fairness, transparency, dignity, and harm avoidance must also be considered.

**2. Differentiate ethics from legal compliance.**
Legal compliance asks whether a data practice follows applicable laws and regulations. Ethical evaluation goes further and considers fairness, transparency, human dignity, and possible social harm. A legally permitted activity may still be irresponsible when it violates reasonable expectations or creates unnecessary risks. For example, a company may legally purchase customer profiles from a data broker, yet customers never expected their information to be used for employee screening. Ethics therefore asks what *should* be done, not only what is *allowed*.

**3. Explain informed consent and voluntary participation.**
Informed consent communicates the project's purpose, procedures, risks, benefits, and intended data uses. The information must be understandable and presented before participation begins, and consent must reflect a meaningful choice rather than hidden or confusing agreement. Participants must be able to withdraw. Voluntary participation means refusal does not produce unfair punishment or loss of essential services. Power relationships can create pressure even without a direct threat, so special care is required when data collectors have authority over participants, such as a professor recruiting their own class.

**4. Explain the ethical issues in social-media data collection.**
Social-media content may be publicly visible while still belonging to a particular social context. Large-scale extraction can expose users to attention and consequences they did not anticipate. Ethical collection therefore considers identifiability, quotation, consent, platform rules, and potential harm. Publishing a user's exact post, for example, can let a search engine reveal the person's identity even after the username is removed. Technical accessibility does not establish ethical or legal permission, so an API-first approach and data minimization are important safeguards.

**5. Explain privacy, confidentiality, and anonymity, and the safeguards that protect them.**
Privacy concerns a person's control over access to personal life and information. Confidentiality concerns how authorized holders protect information from unauthorized disclosure. Anonymity exists when an individual's identity is neither collected nor reasonably connected to the data. These are protected through administrative safeguards such as policies and training, physical safeguards such as locked storage, and technical safeguards such as authentication, encryption, access logging, backups, and network protection. Access control and least privilege limit data to authorized people and approved purposes.

**6. Explain anonymization, pseudonymization, and reidentification risk.**
Anonymization removes or transforms identifying information so individuals are not reasonably identifiable. Direct identifiers alone are not the only concern, because combinations of indirect variables such as age, location, and event dates can reveal identity. Pseudonymization replaces direct identifiers with codes or artificial identifiers, but the information can still be linked to individuals when the separate identification key is available, so pseudonymized records remain sensitive and require access control. Reidentification risk assessment must also consider other datasets that could be combined with the released information.

**7. Explain the ethical responsibilities in data analysis.**
Ethical analysis begins with a responsible research question and a predetermined analytical plan that identifies variables, methods, comparisons, exclusions, and evaluation measures before results are known. Preregistration distinguishes confirmatory analysis from later exploratory discoveries. Analysts must avoid data dredging, p-hacking, multiple-comparison false discoveries, selective reporting, and cherry-picking. They must report uncertainty and limitations, separate training from testing to prevent data leakage, and keep conclusions within the level supported by the design and evidence. Correlation must not be presented as causation without an appropriate design.

**8. Explain bias in data science and its main types.**
Bias is a systematic tendency that moves data, analysis, decisions, or conclusions away from an appropriate standard. It can enter through collection, measurement, labeling, modeling, interpretation, or deployment, and it does not always involve intentional prejudice. Historical bias occurs when data reflect past discrimination; selection bias occurs when included observations differ from the intended population; measurement bias occurs when variables are recorded differently across groups; label bias comes from human judgment or unclear instructions; representation bias occurs when groups have insufficient examples; aggregation bias applies one model to groups with different patterns; and proxy discrimination uses apparently neutral variables to reproduce protected-characteristic decisions.

**9. Explain fairness trade-offs and fairness auditing.**
Fairness can refer to equal selection rates, equal opportunity, balanced error rates, or consistency. Different definitions emphasize different harms and may produce conflicting evaluations. Improving one fairness measure can reduce predictive performance, and these trade-offs cannot be resolved through mathematics alone because they involve values and consequences. A fairness audit examines project purpose, data representation, variables, model performance, and real-world outcomes; it documents affected groups and investigates unequal errors. Auditing continues after deployment because populations and conditions change.

**10. Explain ethical data visualization.**
Ethical visual encoding represents data through position, length, shape, size, color, or area without exaggerating or minimizing values. Position and length support more precise comparisons than decorative area or volume. Chart type must match the data: bar charts for category comparisons, line charts for ordered trends, scatterplots for relationships, and histograms for distributions. Bar charts generally require a zero baseline, truncated axes must not exaggerate, percentages need a clear denominator, and time periods must be relevant. Ethical visualization also uses accessible color palettes, labels, patterns, and text, avoids 3D distortion, displays uncertainty, normalizes choropleth maps, and discloses sources and limitations.

---

# DRILL E — CODE TRACING / ILLUSTRATION

## Part A (Web Scraping)

1. **Trace a safe request.** Write the sequence: `requests.get()` → `timeout` → `raise_for_status()` → `response.text` → `BeautifulSoup(..., "html.parser")`. Explain what happens at each step and what error is caught by `requests.RequestException`.

2. **Trace repeated extraction.** Given `soup.select("div.quote")`, show how a loop builds a list of dictionaries with `quote`, `author`, and `tags`. Explain why consistent field names matter.

3. **Draw the DOM tree** for a page containing `html`, `head`, `title`, `body`, `h1`, `div`, `span`, and `a`. Label parent, child, sibling, and descendant relationships.

4. **Draw the extraction pipeline** from research question to documentation, and mark where permission checking and validation occur.

5. **Draw the Scrapy architecture**: Spider → Request → Downloader → Response → Parser → Item Pipeline → Dataset. Label where settings, retries, and logging apply.

6. **Trace pagination.** Show the loop that follows `li.next a`, applies `urljoin`, stops when the link is absent, and enforces `maximum_pages`.

7. **Draw the API-first decision flow**: official API → institutional dataset → instructor JSON → HTML scraping only if explicitly permitted. Add a stop condition for private/restricted content.

8. **Trace a YouTube API record extraction.** Starting from `payload["items"]`, show how nested `id.videoId`, `snippet.title`, and `snippet.publishedAt` become a flat record. Explain why `.get()` is used.

9. **Draw the data-quality gate**: schema check → missing values → duplicates → text/date cleaning → validation → storage. Label raw vs processed outputs.

10. **Trace a Scrapy export.** Show the difference between `-O quotes.csv` and `-o quotes.jsonl`, and list the checks required before the export is analysis-ready.

## Part B (Ethics)

1. **Draw the data life cycle** (collection → storage → analysis → sharing → deletion) and write one ethical concern at each stage.

2. **Draw the ethical collection checklist** as a flow: Purpose → Necessity → Consent → Inclusion → Security → Retention → Sharing → Review.

3. **Draw the three safeguard layers** (administrative, physical, technical) with two examples under each.

4. **Draw the PIA flow**: Describe processing → Identify risks → Evaluate severity → Select controls → Document decisions → Review changes.

5. **Draw the data-lineage chain**: Survey form → Raw CSV → Cleaned table → Analytical dataset → Dashboard. Mark where documentation and version control apply.

6. **Draw the bias-entry points**: collection, measurement, labeling, modeling, interpretation, deployment. Give one example at each point.

7. **Draw the fairness-audit flow**: Define purpose → Inspect data and groups → Compare metrics → Investigate causes → Apply safeguards → Monitor outcomes.

8. **Draw two versions of the same bar chart**: one with a zero baseline and one truncated at 94. Explain which is ethical and why.

9. **Draw a chart-selection map**: category → bar; time → line; relationship → scatter; distribution → histogram.

10. **Draw an ethical dashboard footer** containing source, collection date, update date, population, units, exclusions, and analytical notes.

---
---

# FINAL REVIEW — HIGH-PROBABILITY ESSAY TOPICS

1. **Web scraping vs web crawling vs API extraction** — definitions, appropriate tools, examples.
2. **The general extraction pipeline** — nine stages, why permission and documentation matter.
3. **Static vs dynamic pages** — evidence, appropriate method, API-first rule.
4. **Responsible scraping** — robots.txt, request frequency, data minimization, copyright, provenance.
5. **Beautiful Soup workflow** — request → parse → select → extract → store.
6. **Beautiful Soup vs Scrapy** — comparison table and when to use each.
7. **Scrapy architecture and paginated crawling** — spider, downloader, parser, pipeline, `response.follow()`.
8. **API-first social-media extraction** — keys/OAuth, endpoints, JSON, pagination, quotas, data dictionary.
9. **Data quality and validation** — schema, missing values, duplicates, text/date cleaning, raw vs processed.
10. **Why ethics matters / ethics vs legal compliance** — core values and examples.
11. **Informed consent, voluntary participation, vulnerable populations, minors.**
12. **Privacy vs confidentiality vs anonymity** — and the safeguards that protect each.
13. **Anonymization vs pseudonymization vs reidentification risk.**
14. **RA 10173 and the principles of transparency, legitimate purpose, and proportionality.**
15. **Data quality as an ethical responsibility** — accuracy, completeness, consistency, validity, provenance.
16. **Bias types and fairness** — historical, selection, measurement, label, representation, aggregation, proxy.
17. **Fairness trade-offs and fairness audits.**
18. **Correlation vs causation, confounders, p-hacking, selective reporting.**
19. **Ethical visualization** — encoding, baselines, denominators, color, accessibility, uncertainty.
20. **Ethical dashboards and maps** — context, normalization, disclosure.

---

# FINAL REVIEW — DIAGRAM / CODE SHORTLIST

1. Extraction pipeline (9 stages)
2. DOM tree with parent/child/sibling labels
3. Client–server request/response flow
4. Beautiful Soup workflow
5. Scrapy architecture
6. API-first decision flow
7. Data-quality gate
8. Data life cycle
9. Three safeguard layers
10. PIA flow
11. Fairness-audit flow
12. Zero baseline vs truncated axis

---

# TRAP-WORD CHECKLIST (Modified T/F favorites)

- Web scraping and web crawling are the **same** → **FALSE** (crawling follows links)
- Public visibility means scraping is **always allowed** → **FALSE**
- `robots.txt` is the **only** rule → **FALSE** (also terms, privacy, authorization)
- API access automatically grants **ethical permission** → **FALSE**
- An API key is a **user password** → **FALSE** (identifies the application)
- Pseudonymization makes reidentification **impossible** → **FALSE**
- Anonymization removes **all** analytical detail → **FALSE** (it may reduce detail, not necessarily eliminate it)
- `find()` returns **all** matches → **FALSE** (`find_all()` does)
- `raise_for_status()` should be **ignored** → **FALSE**
- `-O` **appends** to an existing file → **FALSE** (`-O` replaces; `-o` is the lowercase output option)
- Scrapy is **only** an HTML parser → **FALSE** (it is a crawling framework)
- A multi-page scraper needs **no page limit** → **FALSE**
- Raw data should be **overwritten** during cleaning → **FALSE**
- Duplicates should be removed **without defining comparison fields** → **FALSE**
- Access denied should be **bypassed** → **FALSE** (stop and review)
- Data ethics is **only** technical accuracy → **FALSE**
- Legal = ethical → **FALSE**
- Transparency requires exposing **all confidential information** → **FALSE**
- Automation removes **human responsibility** → **FALSE**
- Ethics is a **final review** → **FALSE** (life-cycle approach)
- Consent is the **only** lawful basis → **FALSE**
- Pseudonymized data are **no longer sensitive** → **FALSE**
- Encryption **in transit** protects stored files → **FALSE** (that is encryption at rest)
- Keeping data **indefinitely** improves value → **FALSE**
- A breach is **only** accidental deletion → **FALSE** (also unauthorized access, disclosure, alteration, loss, destruction)
- A **large sample** fixes systematic exclusion → **FALSE**
- Correlation **proves** causation → **FALSE**
- Removing a protected attribute **guarantees** fairness → **FALSE** (proxies remain)
- Fairness trade-offs can be solved by **mathematics alone** → **FALSE**
- A **truncated axis** is acceptable to exaggerate → **FALSE**
- **3D charts** improve accuracy → **FALSE**
- **Color alone** is sufficient for accessibility → **FALSE**
- A choropleth of **raw counts** accurately shows risk → **FALSE** (normalize rates)
- Dashboards should **hide unfavorable** information → **FALSE**

---

# ONE-PAGE CRAM SHEET

| Must-Know List | Items |
|---|---|
| **Extraction pipeline** | Research Question → Source Assessment → Permission Check → Data Retrieval → Parsing → Cleaning → Validation → Storage → Documentation |
| **Tool choice** | Static small page = Beautiful Soup; paginated/multi-page = Scrapy; platform/social media = Official API |
| **API-first order** | Official API → Authorized institutional dataset → Instructor JSON → HTML scraping only if permitted |
| **Pre-collection checklist** | Public/authorized source; legitimate purpose; necessary variables; page limit; request delay; no access-control bypass; data-protection plan; source documentation |
| **Core ethical values** | Honesty, fairness, privacy, accountability, transparency, avoidance of harm |
| **Collection checklist** | Purpose → Necessity → Consent → Inclusion → Security → Retention → Sharing → Review |
| **Privacy terms** | Privacy = control; confidentiality = protection from disclosure; anonymity = identity not connected |
| **Safeguards** | Administrative (policies/training), Physical (locked storage), Technical (authentication/encryption/logging) |
| **PIA flow** | Describe processing → Identify risks → Evaluate severity → Select controls → Document decisions → Review changes |
| **Incident response** | Containment → Assessment → Documentation → Recovery → Communication |
| **Bias types** | Historical, selection, measurement, label/annotation, representation, aggregation, proxy discrimination |
| **Fairness audit** | Define purpose → Inspect data/groups → Compare metrics → Investigate causes → Apply safeguards → Monitor outcomes |
| **Visualization rules** | Bar = zero baseline; line = trend; scatter = relationship; histogram = distribution; no 3D distortion; show uncertainty; normalize maps; disclose sources |
| **Key numbers** | HTTP 200/403/404/429/500; 9 pipeline stages; 8 checklist items; 6 core values; 3 safeguard types; 5 incident-response steps |
