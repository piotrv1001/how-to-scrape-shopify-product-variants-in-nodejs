# How to Scrape Shopify Product Variants in Node.js

This example calls our [Shopify Product Scraper](https://apify.com/piotrv1001/shopify-product-scraper) on Apify. It does not implement a scraper from scratch.

## What this example does

- Requests five products from one Shopify storefront
- Waits for the Actor run to finish
- Fetches and prints the default dataset
- Shows why product-level availability is not the same as variant-level availability

## Prerequisites

- Node.js 18 or newer
- An Apify account and API token

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and set `APIFY_TOKEN` to your token. Do not commit `.env`.

## Usage

```bash
npm start
```

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    storeUrl: 'https://www.allbirds.com',
    maxProducts: 5,
    includeBarcodes: false,
};

// Run the Actor and wait for it to finish
const run = await client.actor('piotrv1001/shopify-product-scraper').call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

[`sample-output.json`](./sample-output.json) is abbreviated from our September 24, 2026 five-product run. It shows two sizes of one shoe: the product was available for sale, while one size was not. The full dataset also includes a non-merchandise service add-on, so filter the catalog before analyzing shoes. Prices and availability can change after the sample run.

## Use cases

- Audit product variants by SKU and size
- Check which sizes are unavailable in a storefront snapshot
- Compare variant prices rather than relying on one product summary price
- Prepare a catalog export for later monitoring

## Try the Actor on Apify

**[Open the Shopify Product Scraper on Apify](https://apify.com/piotrv1001/shopify-product-scraper)**

## Related resources

- [How to audit Shopify product variants and availability](https://www.falconscrape.com/blog/how-to-audit-shopify-product-variants-and-availability)

## License

MIT
