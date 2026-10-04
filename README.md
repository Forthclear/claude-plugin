# ForthClear for Claude

![ForthClear](./logo.png)

Connects Claude to [ForthClear](https://forthclear.io), a B2B liquidation
marketplace, over the Model Context Protocol.

Ask Claude to find surplus and overstock pallets, pull condition and
bulk-pricing detail, file a quote request with a real seller, or check on an
order. Sellers can list inventory, answer incoming quotes, and mark orders
shipped from the same conversation.

This plugin contains **no application code**. It is one MCP server entry that
points Claude at ForthClear's hosted server, `https://app.forthclear.io/mcp`,
over HTTPS. Nothing runs on your machine, and no credentials are stored in this
repository.

## Install

From claude.ai or Cowork: open the directory, find **ForthClear**, and add it.
It then also loads in Claude Code.

In Claude Code, before the directory listing is live:

```bash
claude plugin marketplace add forthclear/claude-plugin
claude plugin install forthclear@forthclear
```

## Authentication

Catalog search, product lookup, pricing guidance and quote requests work with
**no key**. A quote request without a key needs a contact email, which is
where the seller replies, and is limited to 5 per hour per IP.

Everything tied to an account needs a key you generate yourself:

- Buyers: <https://app.forthclear.io/settings>
- Sellers: <https://app.forthclear.io/seller/settings>

When you enable the plugin, Claude asks for the optional **ForthClear MCP API
key**. It is stored in your system's secure credential store and sent only to
`app.forthclear.io` in the `X-MCP-API-Key` header. A key only ever sees the data
of the account that created it. You can revoke it from the same settings page.

## Tools

| Tool | Reads or writes | What it does |
| --- | --- | --- |
| `search_inventory` | reads | Search liquidation stock by keyword, category, condition, discount, price |
| `search_inventory_semantic` | reads | Rank listings by similarity to a plain-language description |
| `get_product_details` | reads | Condition, lot size, minimum order, bulk price tiers, seller location |
| `get_pricing_recommendation` | reads | Pricing guidance for a listing |
| `request_quote` | writes | File a bulk quote request with a seller |
| `get_quote_status` | reads | Read back a quote you filed and the seller's reply |
| `quick_buy` | writes · key | Purchase directly where the seller has enabled instant checkout |
| `get_order_status` | reads · key | Status, tracking and inspection deadline for your order |
| `open_dispute` | writes · key | Report a problem with a delivered order |
| `subscribe_to_query` | writes · key | Save a search and be notified of new matching listings |
| `create_listing` | writes · seller | List a pallet or lot |
| `get_quote_requests` | reads · seller | Incoming quote requests on your listings |
| `respond_to_quote` | writes · seller | Accept, counter, or decline a quote |
| `bulk_update_listings` | writes · seller | Change price or minimum order across up to 200 listings |
| `bulk_mark_shipped` | writes · seller | Mark up to 200 orders shipped, with tracking |
| `bulk_export_orders` | reads · seller | Your orders as CSV |
| `seller_inbox_summarise` | reads · seller | Digest of pending quotes, orders at risk of shipping late, reported issues |

Every tool carries MCP annotations (`readOnlyHint`, `destructiveHint`) so
Claude can tell a lookup from a purchase before calling it.

## Try it

- "Find pallets of Grade A consumer electronics under $50 a unit, at least 100 units."
- "Request a quote for 200 units of that listing, delivered to Austin, TX."
- "What's in my ForthClear inbox? Anything about to miss its ship-by date?"

## What this plugin sends, and where

- Every tool call goes to `https://app.forthclear.io/mcp` and nowhere else.
- Each call carries the arguments Claude chose for that tool, such as a search
  query, a product id, or a quote's quantity and delivery location.
- If you set an API key, it goes in the `X-MCP-API-Key` header of those calls.
- Payments run through Stripe Connect escrow. Card details never pass through
  Claude or this plugin.

- Privacy policy: <https://forthclear.io/privacy>
- Terms of service: <https://forthclear.io/terms>
- Support: support@forthclear.io

## License

MIT. See [LICENSE](./LICENSE).
