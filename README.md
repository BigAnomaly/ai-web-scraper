[AI Web Scraper](https://apify.com/crawlworks/ai-web-scraper?fpr=data)

## 🤖 What is AI Web Scraper?

**AI Web Scraper** is the simplest way to extract structured data from any webpage — give it a URL and a plain-English prompt, and it returns clean, structured JSON powered by AI. No CSS selectors, no XPath, no code.

- **Extract anything from any webpage:** product prices, job listings, article content, contact details, reviews, tables — if it's on the page, a prompt can get it out
- **Zero configuration:** just a URL and a plain-English description of what you want — the AI figures out the rest
- **Structured output you control:** optionally provide a JSON schema to get output in exactly the shape your application expects
- **Handle JS-heavy and dynamic sites:** built-in support for JavaScript-rendered pages, lazy-loaded content, and bot-detection avoidance with stealth mode
- **Automate research and monitoring workflows:** replace manual copy-paste tasks with repeatable, schedulable scraping pipelines
- **Integrate with any tool:** connect your scraped data directly to Google Sheets, Zapier, Make, Slack, n8n, or any webhook endpoint

**AI Web Scraper turns any webpage into a structured JSON API — with nothing more than a URL and a sentence describing what you need.**

### What can AI Web Scraper extract?

| 🛒 Product prices and availability | 💼 Job listings and descriptions |
| --- | --- |
| 📰 Article headlines and content | 🏢 Company details and contacts |
| ⭐ Reviews and ratings | 📊 Tables and comparison data |
| 🏷️ Categories and tags | 📅 Dates, deadlines, and events |
| 🔗 Links and URLs | 📋 Any custom data you describe |

For maximum usefulness, AI Web Scraper has the following abilities:

- **Prompt-based extraction:** describe what you want in plain English — no technical knowledge required
- **Schema-controlled output:** provide an optional JSON schema to enforce a specific output structure for downstream processing
- **JavaScript rendering:** fully renders pages before extraction, handling dynamic content, single-page applications, and lazy-loaded elements
- **Stealth mode:** applies anti-fingerprinting patches to bypass bot detection on protected websites
- **Wait for selector:** specify a CSS selector to wait for before extraction, ensuring content is fully loaded on complex pages
- **Flexible proxy support:** use Apify Proxy or bring your own custom proxy URLs for reliable, uninterrupted scraping
- **Integrate with other tools:** use webhooks or Apify's MCP server to connect with n8n, Make, Zapier, and more

---

## ⬇️ Input

The input for AI Web Scraper requires just **two things: a URL and a prompt.** Everything else is optional.

### 🌐 URL

The full URL of the webpage you want to scrape.

**URL examples:**

- `https://www.netflix.com/tudum/articles/netflix-plans-and-pricing`
- `https://www.linkedin.com/jobs/view/4401132879/`
- `https://www.amazon.com/dp/B09G9FPHY6`
- `https://news.ycombinator.com`
- `https://yourcompetitor.com/pricing`

Any publicly accessible webpage works. For pages behind login walls or requiring authentication, combine with your own proxy or session cookies via the proxy configuration.

### 💬 Extraction Prompt

Describe in plain English exactly what data you want to extract from the page. Be as specific or as broad as you need — the AI web scraper adapts to your instruction.

**Prompt examples:**

- `Extract all pricing plans including price, features, and billing cycle`
- `Get the job title, company name, location, salary, and required skills`
- `Extract the product name, current price, original price, star rating, and number of reviews`
- `Pull all the speaker names, talk titles, and session times from this conference schedule`
- `Get every FAQ question and its answer from this page`

More specific prompts yield more precise output. If you want a guaranteed output structure, use the optional Output Schema field alongside your prompt.

### 📐 Output Schema (optional)

Provide a JSON schema to define the exact shape of the extracted data. When provided, the AI scraper enforces this structure in its output — making it ideal for pipelines, databases, and integrations that expect a fixed format.

If omitted, the scraper returns free-form JSON based on what the AI determines is most relevant to your prompt.

**Example output schema for job extraction:**

```
{
  "type": "object",
  "properties": {
    "job_title": { "type": "string" },
    "company": { "type": "string" },
    "location": { "type": "string" },
    "salary": { "type": "string" },
    "employment_type": { "type": "string" }
  }
}
```

### 🛡️ Proxy Configuration

Configure proxy settings to ensure reliable access to websites that restrict scraping. You can use Apify Proxy (residential or datacenter) or provide your own custom proxy URLs. Proxy is optional for most public websites.

### 🥷 Stealth Mode

Enable stealth mode to apply anti-bot fingerprinting patches when scraping websites with aggressive bot detection. Recommended for social media platforms, e-commerce sites, and any page that normally blocks automated access.

**Default:** `false`

### ⏳ Wait For Selector (optional)

A CSS selector that the scraper waits for before capturing the page. Use this on JavaScript-heavy pages where content loads asynchronously after the initial page load.

**Example:** `.product-price` or `#job-description` or `[data-testid="pricing-table"]`

---

## ⬆️ Output

Your results appear in an Apify dataset which you can find in the **Output** or **Storage** tab.

Download as JSON, CSV, Excel, HTML, or view as a table. The `extracted` field contains the structured data returned by the AI scraper based on your prompt.

### JSON

Each run produces one output record per URL containing the original URL, your prompt, and the extracted data nested under the `extracted` key.

---

**Example 1 — Pricing page extraction (Netflix)**

Input:

```
{
  "url": "https://help.netflix.com/en/node/24926/us",
  "prompt": "Extract netflix pricing details",
  "useStealth": false
}
```

Output:

```
{
  "url": "https://help.netflix.com/en/node/24926/us",
  "prompt": "Extract netflix pricing details",
  "extracted": {
    "plans": [
      {
        "name": "Standard with ads",
        "price": "$8.99 / month",
        "features": [
          "Ad-supported, all games and most movies and TV shows are available",
          "Watch on 2 supported devices at a time",
          "Watch in 1080p (Full HD)",
          "Download on 2 supported devices at a time"
        ]
      },
      {
        "name": "Standard",
        "price": "$19.99 / month",
        "features": [
          "Unlimited ad-free movies, TV shows, and games",
          "Watch on 2 supported devices at a time",
          "Watch in 1080p (Full HD)",
          "Download on 2 supported devices at a time",
          "Option to add 1 extra member who doesn't live with you"
        ],
        "extra_member": {
          "price_with_ads": "$7.99 / month",
          "price_without_ads": "$9.99 / month"
        }
      },
      {
        "name": "Premium",
        "price": "$26.99 / month",
        "features": [
          "Unlimited ad-free movies, TV shows, and games",
          "Watch on 4 supported devices at a time",
          "Watch in 4K (Ultra HD) + HDR",
          "Download on 6 supported devices at a time",
          "Option to add up to 2 extra members who don't live with you",
          "Netflix spatial audio"
        ],
        "extra_members": {
          "price_with_ads": "$7.99 / month",
          "price_without_ads": "$9.99 / month"
        }
      }
    ]
  }
}
```

---

**Example 2 — Job listing extraction (LinkedIn)**

Input:

```
{
  "url": "https://www.linkedin.com/jobs/view/4401132879/",
  "prompt": "Extract job details of the job in focus, not any other job listings showing on the page.",
  "useStealth": true
}
```

Output:

```
{
  "url": "https://www.linkedin.com/jobs/view/4401132879/",
  "prompt": "Extract job details of the job in focus, not any other job listings showing on the page.",
  "extracted": {
    "job_title": "Grafana Expert - Fully Remote | Upto $150/hr Hourly",
    "company": "Mercor",
    "location": "New York, NY",
    "posted_time": "11 hours ago",
    "hourly_wage": "$150/hr",
    "employment_type": "Part-time",
    "industries": "Software Development",
    "job_function": "Other",
    "seniority_level": null
  }
}
```

---

## ❓ FAQ

### How does AI Web Scraper work?

AI Web Scraper loads the target webpage using a full browser engine, rendering JavaScript and waiting for dynamic content to appear. It then captures the fully rendered page and sends it to a vision-capable AI model along with your extraction prompt. The AI reads the page the same way a human would and returns the requested data as clean, structured JSON. This approach works on virtually any website — no CSS selectors, no XPath, and no maintenance when sites update their layout.

### What makes AI Web Scraper different from traditional scrapers?

Traditional scrapers rely on brittle CSS selectors or XPath expressions that break whenever a website updates its HTML structure. AI Web Scraper uses a vision AI model to understand the visual and semantic content of a page — exactly like a human analyst would — which means it works even when page layouts change, content is dynamically loaded, or the site has no predictable structure to target.

### Can I control the exact format of the output?

Yes. By providing an optional JSON schema in the Output Schema field, you can define precisely what fields you want, what types they should be, and how the output should be nested. When no schema is provided, the scraper returns free-form JSON based on what the AI determines is most relevant to your prompt. For production pipelines and integrations, using a schema is recommended.

### Does AI Web Scraper handle JavaScript-rendered pages?

Yes. The scraper uses a full browser engine to render pages completely before extraction. It supports JavaScript-heavy sites, single-page applications (SPAs), lazy-loaded content, and pages that require user interactions to reveal data. For pages where content loads asynchronously, use the Wait For Selector input to ensure the target element is present before extraction begins.

### Can I use AI Web Scraper as its own API?

Yes. You can use AI Web Scraper programmatically via the Apify API. You can manage, schedule, and run the Actor, access datasets, monitor performance, and retrieve results. To access the API using Node.js, use the `apify-client` [NPM package](https://apify.com/crawlworks/ai-web-scraper/api/client/nodejs). To access the API using Python, use the `apify-client` [PyPI package](https://apify.com/crawlworks/ai-web-scraper/api/client/python).

For detailed information and code examples, see the [**API tab**](https://apify.com/crawlworks/ai-web-scraper/api) or refer to the [**Apify API documentation**](https://docs.apify.com/api/v2).

### Can I integrate AI Web Scraper with other apps?

Yes. AI Web Scraper can be connected with almost any cloud service or web app thanks to [**integrations**](https://apify.com/integrations) on the Apify platform. You can **integrate your scraped data with Zapier, Slack, Make, Airbyte, GitHub, Google Sheets, Asana, LangChain** and more.

You can also use [**webhooks**](https://docs.apify.com/platform/integrations/webhooks) to carry out an action whenever an event occurs — for example, get a notification whenever AI Web Scraper successfully finishes a run.

### Is it legal to use AI web scraping tools?

Web scraping is legal if you are extracting publicly available data. However, you should **respect boundaries such as personal data** and intellectual property regulations. You should only scrape personal data if you have a legitimate reason to do so, and you should also factor in the Terms of Use of the website you are scraping.

### Will multi-URL support be available?

Yes — support for scraping multiple URLs in a single run is on the roadmap and coming soon. For now, you can use the [Apify API](https://apify.com/crawlworks/ai-web-scraper/api) to run multiple Actor instances in parallel to process batches of URLs simultaneously.

---

## 💬 Your feedback

We're always working on improving our Actors' performance. If you have any technical feedback for AI Web Scraper or simply found a bug, please create an issue on the Actor's [Issues tab](https://console.apify.com/actors/crawlworks~ai-web-scraper/issues) in Apify Console.