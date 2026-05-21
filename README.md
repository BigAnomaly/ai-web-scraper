[AI Web Scraper](https://apify.com/raizen/ai-web-scraper?fpr=data)

# AI Web Scraper

Do you need reliable data for your AI agents, LLM pipelines, or training workflows? The **AI Web Scraper** Actor is your key to fast, flexible, and AI‑friendly web extraction on Apify. Under the hood, it relies on the open‑source [Crawl4AI](https://github.com/unclecode/crawl4ai) engine to handle anything from simple single‑page scrapes to deep multi‑link traversals (BFS/DFS/BestFirst). Whether you want clean markdown, JSON extraction, or LLM summarization, just specify your desired strategy via the Actor’s input UI, and you’re set. This Actor integrates well with Make.com, n8n, and Zapier for AI automation

Below is an overview of each setting you’ll see in the Apify interface and how it affects your crawls.

---

## Quick How-To

1. **Start with URLs**

At a minimum, provide `startUrls` in the UI (or JSON input)—the pages you want to scrape.
2. **Pick a Crawler & Extraction Style**

Choose between various crawl strategies (e.g., BFS or DFS) and extraction methods (simple markdown, LLM-based, JSON CSS, etc.). You can also enable content filtering or deeper link exploration.
3. **Review the Output**

Once the Actor finishes, your results will appear in the Apify Dataset—structured JSON, markdown, or whichever format you chose.

---

## Input Fields Explained

These fields appear in the Actor’s input UI. Customize them to match your use case.

### 1. **startUrls** (Required)

List of pages to scrape. For each entry, just provide `"url"`.

**Example**:

```
{
  "startUrls": [
    { "url": "https://example.com" }
  ]
}
```

### 2. **browserConfig** (Optional)

Configure Playwright’s browser behavior—headless mode, custom user agent, viewport size, etc.

- `browser_type`: “chromium”, “firefox”, or “webkit”
- `headless`: Boolean to run in headless mode
- `verbose_logging`: Extra debug logs
- `ignore_https_errors`: Accept invalid certs
- `user_agent`: E.g. “random” or a custom string
- `proxy`: Proxy server URL
- `viewport_width` / `viewport_height`: Window size
- `accept_downloads`: Whether downloads are allowed
- `extra_headers`: Additional request headers

### 3. **crawlerConfig** (Optional)

Core crawling settings—time limits, caching, JavaScript hooks, or multi‑page concurrency.

- `cache_mode`: “BYPASS” (no cache), “ENABLED”, etc.
- `page_timeout`: Milliseconds to wait for page loads
- `simulate_user`: Stealth by mimicking user actions
- `remove_overlay_elements`: Attempt to remove popups
- `delay_before_return_html`: Extra wait before final extraction
- `wait_for`: Wait time or wait condition
- `screenshot` / `pdf`: Capture screenshot or PDF
- `enable_rate_limiting`: Rate limit large URL lists
- `memory_threshold_percent`: Pause if memory is too high
- `word_count_threshold`: Discard short text blocks
- `css_selector`, `excluded_tags`, `excluded_selector`: Further refine or skip sections of the DOM
- `only_text`: Keep plain text only
- `prettify`: Attempt to clean up HTML
- `keep_data_attributes`: Keep or drop data-* attributes
- `remove_forms`: Strip `<form>` elements
- `bypass_cache` / `disable_cache` / `no_cache_read` / `no_cache_write`: Fine‑grained caching controls
- `wait_until`: E.g. “domcontentloaded” or “networkidle”
- `wait_for_images`: Wait for images to fully load
- `check_robots_txt`: Respect robots.txt?
- `mean_delay`, `max_range`: Introduce a random delay range
- `js_code`: Custom JS to run on each page
- `js_only`: Reuse the same page context without re-navigation
- `ignore_body_visibility`: Include hidden elements
- `scan_full_page`: Scroll from top to bottom for lazy loading
- `scroll_delay`: Delay between scroll steps
- `process_iframes`: Also parse iframes
- `override_navigator`: Additional stealth tweak
- `magic`: Enable multiple advanced anti-bot tricks
- `adjust_viewport_to_content`: Resize viewport to fit content
- `screenshot_wait_for`: Wait time before taking a screenshot
- `screenshot_height_threshold`: Max doc height to screenshot
- `image_description_min_word_threshold`: Filter out images with minimal alt text
- `image_score_threshold`: Remove lower‑score images
- `exclude_external_images`: No external images
- `exclude_social_media_domains`, `exclude_domains`: Avoid these domains entirely
- `exclude_external_links`, `exclude_social_media_links`: Strip external or social media links
- `verbose`: Extra logs
- `log_console`: Show browser console logs?
- `stream`: Stream results as they come in, or wait until done

### 4. **deepCrawlConfig** (Optional)

When you select BFS, DFS, or BestFirst crawling, this config guides link exploration.

- `max_pages`: Stop after crawling this many pages
- `max_depth`: Depth of link‑following
- `include_external`: Follow off‑domain links?
- `score_threshold`: Filter out low‑score links (BestFirst)
- `filter_chain`: Extra link filter rules
- `url_scorer`: If you want a custom approach to scoring discovered URLs

### 5. **markdownConfig** (Optional)

For HTML→Markdown conversions.

- `ignore_links`: Skip anchor links
- `ignore_images`: Omit markdown images
- `escape_html`: Turn `<div>` into `&lt;div&gt;`
- `skip_internal_links`: Remove same‑page anchors
- `include_sup_sub`: Preserve `<sup>/<sub>` text
- `citations`: Put footnotes at bottom of file
- `body_width`: Wrap lines at N chars
- `fit_markdown`: Use advanced “fit” mode if also using a filter

### 6. **contentFilterConfig** (Optional)

Prune out nav bars, sidebars, or extra text using “pruning,” “bm25,” or a second LLM filter.

- `type`: e.g. “pruning”, “bm25”
- `threshold`: Score cutoff
- `min_word_threshold`: Minimum words to keep
- `bm25_threshold`: BM25 filter param
- `apply_llm_filter`: If `true`, do a second pass with an LLM
- `semantic_filter`: Keep only text about a certain topic
- `word_count_threshold`: Another word threshold
- `sim_threshold`, `max_dist`, `top_k`, `linkage_method`: For advanced clustering

### 7. **userAgentConfig** (Optional)

Rotate or fix your user agent.

- `user_agent_mode`: “random” or “fixed”
- `device_type`: “desktop” or “mobile”
- `browser_type`: e.g. “chrome”
- `num_browsers`: If rotating among multiple agents

### 8. **llmConfig** (Optional)

For LLM-based extraction or filtering.

- `provider`: e.g. “openai/gpt-4”, “groq/deepseek-r1-distill-llama-70b”
- `api_token`: Model’s API key
- `instruction`: Prompt the LLM about how to parse or summarize
- `base_url`: For custom endpoints
- `chunk_token_threshold`: Big pages → chunk them
- `apply_chunking`: Boolean for chunking
- `input_format`: “markdown” or “html”
- `temperature`, `max_tokens`: Standard LLM config

### 9. **session_id** (Optional)

Provide a session ID to reuse browser context across multiple runs (logins, multi-step flows, etc.).

### 10. **extractionStrategy** (Optional)

Pick one:

- `SimpleExtractionStrategy`: Simple HTML→Markdown.
- `LLMExtractionStrategy`: Let an LLM parse or summarize.
- `JsonCssExtractionStrategy` / `JsonXPathExtractionStrategy`: Provide a schema to produce structured JSON.

### 11. **crawlStrategy** (Optional)

- `SimpleCrawlStrategy`: Just the given start URLs
- `BFSDeepCrawlStrategy`: Breadth-first approach
- `DFSDeepCrawlStrategy`: Depth-first approach
- `BestFirstCrawlingStrategy`: Score links, pick the best first

### 12. **extractionSchema** (Optional)

If using CSS/XPath extraction.

- `name`: Your extraction scheme name
- `baseSelector`: Parent selector for repeated elements
- `fields`: Each with `name`, `selector`, `type`, and optional `attribute`

**Example**:

```
"extractionSchema": {
  "name": "Custom Extraction",
  "baseSelector": "div.article",
  "fields": [
    { "name": "title", "selector": "h1", "type": "text" },
    { "name": "link", "selector": "a", "type": "attribute", "attribute": "href" }
  ]
}
```

---

## Usage Examples

### Minimal

```
{
  "startUrls": [
    { "url": "https://example.com" }
  ]
}
```

Scrapes a single page in headless mode with standard markdown output.

### JSON CSS Extraction

```
{
  "startUrls": [
    { "url": "https://news.ycombinator.com/" }
  ],
  "extractionStrategy": "JsonCssExtractionStrategy",
  "extractionSchema": {
    "name": "HackerNews",
    "baseSelector": "tr.athing",
    "fields": [
      {
        "name": "title",
        "selector": ".titleline a",
        "type": "text"
      },
      {
        "name": "link",
        "selector": ".titleline a",
        "type": "attribute",
        "attribute": "href"
      }
    ]
  }
}
```

Generates a JSON array, each object containing “title” and “link.”

---

## Pro Tips

- **Deep crawling**: If you want BFS or DFS, set `crawlStrategy` to “BFSDeepCrawlStrategy” or “DFSDeepCrawlStrategy” and configure `deepCrawlConfig`.
- **Content filtering**: Combine `contentFilterConfig` with `extractionStrategy` for maximum clarity and minimal noise.
- **LLM-based**: Choose “LLMExtractionStrategy” plus `llmConfig` for advanced summarization or structured data. Great for building AI pipelines.

Thanks for trying out the **AI Web Scraper**—enjoy harnessing rich, clean data for your Apify-based AI solutions!