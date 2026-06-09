[Ceneo Scraper](https://apify.com/studio-amba/ceneo-scraper?fpr=data)

# Ceneo Scraper -- Polish Price Comparison, Product Data & Store Offers

Extract product listings, price comparisons in PLN, ratings, reviews, and multi-retailer offer data from [Ceneo.pl](https://www.ceneo.pl) -- Poland's largest and most trusted price comparison platform, aggregating prices from thousands of online shops for millions of products.

## What is Ceneo Scraper?

Ceneo is to Poland what Google Shopping is to the rest of Europe -- except bigger and more deeply integrated into Polish online shopping culture. With over 30 million monthly visitors, Ceneo is the default starting point for Polish consumers comparing prices before any online purchase. It covers every product category imaginable: electronics, home appliances, fashion, beauty, automotive parts, toys, garden equipment, and much more.

What makes Ceneo uniquely valuable is its depth: for any single product, Ceneo shows offers from dozens of competing retailers with individual prices, shipping costs, and seller ratings. This actor extracts that aggregated intelligence at scale.

Here is what people build with it:

- **Polish e-commerce pricing** -- online retailers operating in Poland monitor Ceneo prices daily to maintain competitive positioning against hundreds of rival shops.
- **Brand distribution analysis** -- consumer brands track which Polish retailers list their products, at what price points, and with how many total offers.
- **Market entry intelligence** -- companies planning to sell in Poland use Ceneo data to understand price expectations, popular categories, and competitive dynamics.
- **Consumer sentiment analysis** -- Ceneo's rating and review data provides a massive source of Polish consumer product opinions for market researchers and product teams.
- **Deal detection** -- deal aggregation services and browser extensions use Ceneo pricing data to surface the best offers for Polish shoppers.
- **Dropshipping research** -- e-commerce entrepreneurs use Ceneo's multi-retailer pricing to identify products with healthy margins and reliable supply across Polish suppliers.

## What data does Ceneo Scraper extract?

Each product record includes:

- :package: **Product name** -- full product title in Polish
- :label: **Brand** -- manufacturer brand
- :zloty: **Price** -- lowest available price in PLN (Polish zloty)
- :chart_with_downwards_trend: **Lowest and highest prices** -- price range across all retailers
- :department_store: **Offers count** -- number of shops selling this product
- :barcode: **EAN code** -- GTIN-13 barcode for product identification
- :id: **SKU and product ID** -- Ceneo's internal identifiers
- :white_check_mark: **Stock status** -- whether the product is available for purchase
- :star: **Rating** -- aggregate user rating
- :speech_balloon: **Review count** -- number of consumer reviews
- :camera: **Image** -- product image URL
- :page_facing_up: **Description** -- product description text
- :file_folder: **Category and breadcrumbs** -- full category path from Ceneo's taxonomy
- :link: **URL** -- direct link to the Ceneo product page

## How to scrape Ceneo.pl

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `searchQuery` | String | No | Search by keyword (Polish): `"sluchawki"` (headphones), `"laptop"`, `"telewizor"` |
| `categoryUrl` | String | No | A Ceneo.pl category or search URL to scrape |
| `maxResults` | Integer | No | Maximum products to return (default: 100, max: 50,000) |
| `proxyConfiguration` | Object | No | Proxy settings for large runs |

**Tips:**

- Use **Polish keywords** for comprehensive results. `"sluchawki bezprzewodowe"` (wireless headphones) returns more results than "wireless headphones".
- For category-wide scraping, browse ceneo.pl to find the category page URL and paste it into `categoryUrl`.
- The actor handles pagination automatically via Ceneo's "nastepna" (next) page links.
- Provide either `searchQuery` OR `categoryUrl`. If neither is provided, the default search is `"sluchawki"`.

## Output

```
{
    "name": "Apple AirPods Pro 2 (USB-C)",
    "brand": "Apple",
    "price": 949,
    "currency": "PLN",
    "lowestPrice": 949,
    "highestPrice": 1299,
    "offersCount": 47,
    "ean": "0195949052552",
    "sku": "MTJV3ZM/A",
    "productId": "138927451",
    "inStock": true,
    "rating": 4.8,
    "reviewCount": 2341,
    "imageUrl": "https://image.ceneo.pl/data/products/138927451/i-apple-airpods-pro-2.jpg",
    "description": "Sluchawki Apple AirPods Pro 2 z aktywna redukcja szumow, dzwiekiem przestrzennym i etui z USB-C...",
    "category": "Sluchawki bezprzewodowe",
    "categories": ["Elektronika", "Audio", "Sluchawki", "Sluchawki bezprzewodowe"],
    "url": "https://www.ceneo.pl/138927451",
    "scrapedAt": "2026-04-03T16:00:00.000Z"
}
```

## How much does it cost?

Ceneo Scraper uses CheerioCrawler with efficient listing-then-detail page traversal:

| Volume | Estimated CUs | Estimated Cost |
| --- | --- | --- |
| 100 products | ~0.02 | ~$0.01 |
| 500 products | ~0.10 | ~$0.05 |
| 1,000 products | ~0.20 | ~$0.10 |
| 5,000 products | ~0.90 | ~$0.45 |

The actor first collects product IDs from listing pages, then visits each product page for full details including JSON-LD structured data.

## Can I integrate?

Connect Polish pricing data to your business tools:

- **Google Sheets** -- build a live Polish market price tracker for your product portfolio
- **Slack** -- get alerts when competitor prices change on Ceneo
- **Zapier / Make** -- automate competitive pricing updates for your Polish e-shop
- **Webhooks** -- feed real-time price data to your dynamic pricing engine
- **BigQuery / PostgreSQL** -- build a historical Polish market price database
- **Power BI / Tableau** -- visualise price distributions and competitive dynamics

## Can I use it as an API?

Yes. Integrate Polish price comparison data into your pipeline:

**Python:**

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")

run = client.actor("studio-amba/ceneo-scraper").call(run_input={
    "searchQuery": "smartfon",
    "maxResults": 200,
})

for product in client.dataset(run["defaultDatasetId"]).iterate_items():
    offers = product.get('offersCount', '?')
    low = product.get('lowestPrice', '?')
    high = product.get('highestPrice', '?')
    print(f"{product['name']} | {low}-{high} PLN | {offers} sklepy")
```

**JavaScript:**

```
import { ApifyClient } from "apify-client";

const client = new ApifyClient({ token: "YOUR_API_TOKEN" });

const run = await client.actor("studio-amba/ceneo-scraper").call({
    searchQuery: "smartfon",
    maxResults: 200,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
const withRatings = items.filter((p) => p.rating >= 4.5 && p.reviewCount > 100);
console.log(`${withRatings.length} highly-rated smartphones on Ceneo:`);
withRatings.forEach((p) => {
    console.log(`  ${p.name} | ${p.price} PLN | ${p.rating}/5 (${p.reviewCount} reviews)`);
});
```

## FAQ

**Is Ceneo only for electronics?**
No. Ceneo covers virtually every product category: electronics, home appliances, fashion, beauty, sports, automotive, garden, children's products, pet supplies, and more. It is a universal price comparison platform.

**Are prices in PLN or EUR?**
All prices are in Polish zloty (PLN). Ceneo is a Polish platform for the Polish market.

**Can I see individual store prices?**
The actor extracts the aggregate lowest price, highest price, and total number of offers. Individual per-retailer prices would require deeper scraping of the offers tab on each product page.

**How reliable are the ratings?**
Ceneo's review system is one of the most trusted in Poland. Reviews are tied to verified purchases when possible. The rating and review count come from Ceneo's structured data.

**Does it handle Polish characters in search?**
Yes. Polish characters (Polish diacritics) work correctly in search queries. The actor encodes them properly for the URL.

**Can I scrape specific product pages by ID?**
Use `categoryUrl` with a direct Ceneo product URL like `https://www.ceneo.pl/138927451` to scrape a specific product.

**How large is Ceneo's product database?**
Ceneo tracks millions of products across thousands of categories. The platform aggregates offers from over 30,000 online shops in Poland, making it by far the most comprehensive pricing source for the Polish e-commerce market.

**Can I use Ceneo data for MAP (Minimum Advertised Price) monitoring?**
Yes. By tracking the lowest price and highest price for a specific EAN across all retailers, you can identify shops that may be undercutting your MAP policy. Schedule the actor daily for continuous monitoring.

**What happens if a product has no offers?**
Products with zero active offers may still have a Ceneo page with historical data. The `inStock` field will be `false` and `offersCount` will be `0` for such products.

## Limitations

- Targets ceneo.pl (Poland) only. Ceneo does not operate in other countries.
- Individual retailer offer details (per-shop prices, shipping costs, seller ratings) require deeper page scraping beyond the current implementation.
- Ceneo may serve different content to different user-agents. The actor uses standard browser headers but some dynamic content may not be accessible.
- Very broad searches (e.g., single-word queries) may return thousands of results. Use `maxResults` to control scope.
- Product descriptions and categories are in Polish. No translation is performed.

## Related price comparison and European scrapers

- [Prisjakt Scraper](https://apify.com/studio-amba/prisjakt-scraper) -- Sweden's largest price comparison platform
- [Apotea Scraper](https://apify.com/studio-amba/apotea-scraper) -- Swedish online pharmacy products and prices
- [Norauto Scraper](https://apify.com/studio-amba/norauto-scraper) -- French automotive retail prices
- [AutoVlan Scraper](https://apify.com/studio-amba/autovlan-scraper) -- Belgian used car market data

## Your feedback

Want individual retailer prices, review text extraction, or support for specific product categories? Open an issue on GitHub or contact us through the Apify platform. We improve based on what users actually need.