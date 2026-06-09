[Ceneo Scraper](https://apify.com/trev0n/ceneo-scraper?fpr=data)

# Contact

If you encounter any issues or need to exchange information, please feel free to contact us through the following link:
[My profile](https://apify.com/trev0n)

# What does Ceneo.pl Scraper do?

## Introduction

Ceneo.pl is Poland's largest and most comprehensive price comparison website, aggregating product listings from thousands of online retailers across all major consumer categories including electronics, home appliances, fashion, gaming, books, and more. As the primary destination for Polish consumers seeking the best prices and product information, Ceneo.pl represents an invaluable source of market intelligence for businesses, researchers, and developers seeking comprehensive pricing data, product specifications, and competitive analysis across Poland's e-commerce landscape.

The challenge lies in manually navigating through thousands of product listings, comparing prices across dozens of retailers, and collecting detailed product specifications for market analysis. Our Ceneo.pl Scraper eliminates this time-consuming process by automating data extraction from both search/category pages and individual product pages, providing structured access to product listings with multi-store pricing, detailed specifications, customer ratings, and shop offers that can drive competitive intelligence and pricing strategies.

## Scraper Overview

The Ceneo.pl Scraper is designed to extract comprehensive product and pricing data from Poland's leading price comparison platform with precision and reliability. Built with Playwright and Crawlee, this scraper handles the complexities of modern e-commerce navigation including dual URL type support, automatic pagination, lazy-loaded content, and multi-store price aggregation while delivering clean, structured product data.

Key advantages include dual URL support for both search pages and product details, automatic pagination handling to traverse multiple result pages, comprehensive multi-store price extraction from all available retailers, detailed product specifications and technical data extraction, customer ratings and review counts collection, and configurable concurrency controls for optimal performance. The scraper is particularly valuable for retailers monitoring competitive pricing strategies, market researchers analyzing product availability and pricing trends, e-commerce businesses identifying market opportunities and pricing gaps, and data analysts building comprehensive product databases with multi-retailer pricing.

Target users include retail price analysts, competitive intelligence teams, market research firms, e-commerce entrepreneurs monitoring competitor pricing, pricing strategists tracking market dynamics, dropshipping businesses identifying profitable products, and data scientists working with Polish e-commerce market data.

## Input and Output Specifications

### Example URLs

**Example URL 1 (Search/Category):** [https://www.ceneo.pl/Gry;szukaj-battlefield+6](https://www.ceneo.pl/Gry;szukaj-battlefield+6)

**Example URL 2 (Category):** [https://www.ceneo.pl/Elektronika](https://www.ceneo.pl/Elektronika)

**Example URL 3 (Product Detail):** [https://www.ceneo.pl/186696977](https://www.ceneo.pl/186696977)

---

### Input Format

The scraper accepts JSON configuration with the following parameters:

```
{
  "startUrls": [
    { "url": "https://www.ceneo.pl/Gry;szukaj-battlefield+6" },
    { "url": "https://www.ceneo.pl/Elektronika" },
    { "url": "https://www.ceneo.pl/186696977" }
  ],
  "maxRequestsPerCrawl": 100,
  "maxConcurrency": 5,
  "scrapeProductDetails": true,
  "proxyConfiguration": {
    "useApifyProxy": false
  }
}
```

**startUrls** - An array containing Ceneo.pl URLs to scrape. Accepts two URL formats: (1) search/category pages for extracting product listings, and (2) individual product detail pages for comprehensive product data. The scraper automatically detects URL type and applies appropriate extraction logic.

**maxRequestsPerCrawl** - Maximum number of pages to scrape during the crawl (default: 100, set to 0 for unlimited). Controls the total scope of the scraping operation including both listing pages and product detail pages.

**maxConcurrency** - Maximum number of pages to process simultaneously (default: 5, range: 1-20). Higher values increase scraping speed but consume more resources. Recommended: 3-5 for stable performance.

**scrapeProductDetails** - Boolean flag controlling automatic product detail extraction (default: true). When enabled and scraping search/category pages, the scraper automatically navigates to each product page to collect comprehensive product information including specifications, all shop offers, and detailed descriptions.

**proxyConfiguration** - Optional proxy configuration object. Set `useApifyProxy: true` to use Apify Proxy for enhanced reliability and reduced blocking risks. Highly recommended for large-scale scraping operations.

---

### Output

You get the output from the Ceneo.pl Scraper stored in a dataset. The scraper produces two types of results depending on the source page type.

**Search Result Output Example:**

```
{
  "resultType": "search_result",
  "url": "https://www.ceneo.pl/Gry;szukaj-battlefield+6",
  "productId": "186696977",
  "productUrl": "https://www.ceneo.pl/186696977",
  "name": "Battlefield 2042 (PC)",
  "price": "99,99 zł",
  "priceValue": 99.99,
  "image": "https://image.ceneo.pl/data/products/186696977/i-battlefield-2042-pc.jpg",
  "rating": "4.5/5",
  "offersCount": "15 ofert",
  "scrapedAt": "2024-11-19T22:30:00.000Z"
}
```

**Product Detail Output Example:**

```
{
  "resultType": "product",
  "url": "https://www.ceneo.pl/186696977",
  "productId": "186696977",
  "name": "Battlefield 2042 (PC)",
  "mainImage": "https://image.ceneo.pl/data/products/186696977/i-battlefield-2042-pc.jpg",
  "images": [
    "https://image.ceneo.pl/data/products/186696977/i-battlefield-2042-pc.jpg",
    "https://image.ceneo.pl/data/products/186696977/f-battlefield-2042-screenshot-1.jpg",
    "https://image.ceneo.pl/data/products/186696977/f-battlefield-2042-screenshot-2.jpg"
  ],
  "categories": ["Gry", "Gry PC", "Akcji"],
  "rating": "4.5/5",
  "ratingValue": 4.5,
  "reviewsCount": "234 opinie",
  "productDescription": "Battlefield 2042 to futurystyczna strzelanka pierwszoosobowa osadzona w niedalekiej przyszłości, gdzie gracze walczą na ogromnych mapach w trybie wieloosobowym dla maksymalnie 128 graczy.",
  "specifications": {
    "Platforma": "PC",
    "Gatunek": "Akcji, FPS",
    "Tryb gry": "Singleplayer, Multiplayer",
    "Wydawca": "Electronic Arts",
    "Data premiery": "19 listopada 2021"
  },
  "offers": [
    {
      "shopName": "Sklep1.pl",
      "price": "99,99 zł",
      "priceValue": 99.99,
      "delivery": "Darmowa dostawa",
      "offerUrl": "https://www.ceneo.pl/Click/Offer/?e=..."
    },
    {
      "shopName": "Sklep2.pl",
      "price": "104,90 zł",
      "priceValue": 104.9,
      "delivery": "9,99 zł",
      "offerUrl": "https://www.ceneo.pl/Click/Offer/?e=..."
    }
  ],
  "lowestPrice": 99.99,
  "scrapedAt": "2024-11-19T22:30:00.000Z"
}
```

The scraper returns structured data with comprehensive fields serving specific business intelligence and market analysis purposes:

**resultType** - Result type identifier ("search_result" or "product"), enabling easy filtering and processing of different data structures in your pipeline.

**url** - The source page URL where the data was extracted, essential for data provenance tracking and crawl verification.

**productId** - Unique Ceneo.pl product identifier extracted from the URL, crucial for deduplication, product matching across different scraping runs, and building product tracking systems.

**productUrl** - Direct link to the product detail page on Ceneo.pl, enabling seamless navigation between listing and detail data, and supporting follow-up scraping workflows.

**name** - Complete product title as displayed on Ceneo.pl, fundamental for product identification, catalog matching, and search indexing.

**price** - Lowest available price as displayed in search results (text format), providing quick price reference in human-readable format.

**priceValue** - Numeric representation of the price in PLN, essential for price comparison algorithms, statistical analysis, and automated pricing strategies.

**lowestPrice** - The absolute lowest price from all available shop offers (product details only), critical for competitive pricing analysis and identifying the best market price.

**image** / **mainImage** - Primary product image URL, enabling visual catalog creation, automated image downloads for product databases, and visual comparison tools.

**images** - Array of all available product images (product details only), valuable for comprehensive visual documentation, multi-angle product views, and enhanced catalog presentations.

**rating** - Average customer rating as displayed (text format with "/5"), providing quick quality assessment in human-readable format.

**ratingValue** - Numeric rating value (product details only), essential for quality analysis, customer satisfaction scoring, and product ranking algorithms.

**reviewsCount** - Number of customer reviews (product details only), indicating product popularity and review sample size for reliability assessment.

**offersCount** - Number of shop offers available (search results only), indicating market competition and product availability across retailers.

**categories** - Array of product category hierarchy from breadcrumb navigation (product details only), enabling category-based analysis, product classification, and market segmentation.

**productDescription** - Detailed product description (product details only), valuable for product understanding, content analysis, and automated product documentation.

**specifications** - Object containing technical product specifications as key-value pairs (product details only), critical for detailed product comparison, filtering, and technical analysis across similar products.

**offers** - Array of all shop offers with detailed pricing and delivery information (product details only), enabling comprehensive multi-retailer price comparison, delivery cost analysis, and retailer performance tracking. Each offer includes shopName, price (text), priceValue (numeric), delivery cost information, and direct offer URL.

**scrapedAt** - ISO 8601 timestamp of data extraction, essential for temporal analysis, price history tracking, and data freshness validation.

---

## Usage Guide

Begin by identifying target search queries, product categories, or specific product IDs on Ceneo.pl. The scraper accepts two distinct URL types: (1) search and category pages for collecting product listings, and (2) individual product pages for comprehensive product details including multi-store pricing.

Configure the input JSON with your target URLs and desired crawl parameters. Set `maxRequestsPerCrawl` to control the total number of pages scraped - this includes both listing pages and product detail pages when `scrapeProductDetails` is enabled. Adjust `maxConcurrency` based on your resource availability and desired speed (recommended: 3-5 for stable performance).

The scraper automatically handles:

- Dual URL type detection (search/category vs. product detail pages)
- Multi-page pagination with intelligent traversal on category/search pages
- Lazy-loaded product images and dynamic content
- Product specification extraction from structured data tables
- Multi-store offer extraction with pricing and delivery information
- Price value normalization (converting "99,99 zł" to numeric 99.99)
- Category hierarchy extraction from breadcrumb navigation
- Automatic product detail scraping when enabled

**Best Practices:**

- Start with small `maxRequestsPerCrawl` values (20-50 pages) to validate URL correctness and output quality before scaling to larger operations
- Use `scrapeProductDetails: true` when comprehensive product data including all shop offers and specifications is required
- Set `scrapeProductDetails: false` when only basic listing data (product names, IDs, lowest prices) is sufficient for your use case
- Enable proxy configuration (`useApifyProxy: true`) for large-scale operations to avoid IP blocking
- Monitor the ratio of listing pages to product pages in your request count to optimize `maxRequestsPerCrawl` settings
- Use specific category URLs rather than broad searches for focused data collection
- Combine search URLs with filters (price ranges, brands) to target specific market segments

**Common scenarios:**

- **Competitive price monitoring across retailers** - Use product detail URLs or enable `scrapeProductDetails` to collect all shop offers with pricing and delivery costs for comprehensive multi-retailer comparison
- **Market research across product categories** - Use category URLs to collect product listings from specific segments (electronics, gaming, home appliances) for market composition analysis
- **Price tracking for specific products** - Use product detail URLs with scheduled runs to track price changes over time from multiple retailers
- **Product discovery and catalog building** - Search for specific brands, models, or keywords and enable `scrapeProductDetails` to build comprehensive product databases with specifications
- **Availability monitoring** - Track the number of offers (`offersCount`) and available shops to monitor product availability and market competition

**Common errors and solutions:**

- **No products found on search pages** - Verify the search URL returns product listings when accessed manually; some searches may have zero results or require authentication
- **Missing product details** - Some products may have incomplete data on Ceneo.pl; the scraper extracts all available fields but some may be null for certain products
- **Pagination not advancing** - Check that you haven't hit `maxRequestsPerCrawl` limit; increase this value to scrape more pages
- **Missing shop offers** - Some products may only show offers after clicking "Pokaż wszystkie oferty" button; the scraper handles this automatically but some products may have limited offer data publicly visible
- **Proxy errors or blocking** - Enable `useApifyProxy: true` in proxy configuration to avoid IP-based blocking on large-scale scraping operations

Monitor scraper performance through detailed logging including:

- URL type detection (search page vs. product page)
- Products found per listing page
- Pagination advancement status
- Product detail page requests
- Shop offers extracted per product
- Request success/failure rates

---

## Benefits and Applications

The scraper delivers significant time savings by automating manual price comparison and product data collection across thousands of retailers aggregated on Ceneo.pl. For a typical competitive analysis task covering 500 products with multi-store pricing from 10-20 retailers each, manual collection could require weeks of work, while automated scraping completes in hours.

**Real-world applications include:**

- **Multi-retailer competitive price monitoring** for tracking pricing strategies across all Polish e-commerce retailers offering specific products
- **Dynamic pricing optimization** by analyzing competitor prices and adjusting your pricing strategy based on market data
- **Product availability tracking** to monitor stock levels and retailer participation across product categories
- **Market gap analysis** identifying underserved product categories or pricing opportunities
- **Retailer performance analysis** tracking which shops offer the best prices, delivery terms, and market presence
- **Price elasticity research** collecting historical pricing data to understand demand sensitivity
- **Product specification databases** building comprehensive catalogs with technical specifications for comparison engines
- **Review and rating analysis** collecting customer satisfaction data for quality assessment and product selection

**Business value extends to:**

- Identifying pricing patterns and promotional strategies across retailers and product categories
- Monitoring new product launches and market entries through category tracking
- Analyzing retailer market share and competitive positioning by tracking offer counts and pricing
- Supporting pricing decisions with comprehensive multi-retailer market data
- Generating competitive intelligence reports for specific product segments
- Building training datasets for pricing algorithms and recommendation systems
- Tracking seasonal trends and promotional cycles through periodic scraping
- Enabling automated price matching strategies based on real-time competitor data

**Integration opportunities:**

- Export to time-series databases for price history tracking and trend analysis
- Feed into business intelligence dashboards for real-time competitive monitoring
- Power price comparison and recommendation engines on your own platforms
- Integrate with pricing automation systems for dynamic price adjustments
- Combine with inventory systems for automated repricing based on competition
- Use with data analytics tools for market basket analysis and product correlation studies

---

## Conclusion

The Ceneo.pl Scraper transforms tedious manual price comparison and product research into automated, structured data collection from Poland's leading price comparison marketplace. Whether you're conducting competitive price monitoring across multiple retailers, building comprehensive product databases, analyzing market trends, or optimizing your pricing strategy, this scraper provides the reliable foundation for data-driven e-commerce decision making.

The dual URL support, comprehensive multi-store price extraction, and detailed product specification collection make this scraper ideal for both focused competitive analysis projects and large-scale market intelligence operations across Poland's e-commerce landscape.

---

## Your Feedback

We are always working to improve Actors' performance. So, if you have any technical feedback about Ceneo.pl Scraper or simply found a bug, please create an issue on the Actor's Issues tab in Apify Console.