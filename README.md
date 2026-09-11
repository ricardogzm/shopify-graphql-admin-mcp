# shopify-graphql-admin-mcp

MCP server for Shopify's Admin GraphQL API. It reads the live schema when it starts, so the tools use the selected API version without bundled schema files or embeddings.

## Features

- **Raw GraphQL execution.** Run any query or mutation against the Admin API.
- **Live schema introspection.** Search and inspect the full GraphQL schema, including more than 2,700 types, from your AI assistant.
- **29 convenience tools.** Use typed operations for products, collections, metaobjects, metafields, customers, orders, and inventory without writing GraphQL.
- **Two auth modes.** Use OAuth client credentials from the Dev Dashboard or legacy access tokens (`shpat_`).
- **Automatic token refresh.** OAuth tokens refresh before they expire.

## Quick Start

```bash
# With a legacy access token
npx shopify-graphql-admin-mcp --store mystore.myshopify.com --access-token shpat_xxxxx

# With OAuth client credentials (Dev Dashboard app)
npx shopify-graphql-admin-mcp --store mystore.myshopify.com --client-id YOUR_ID --client-secret YOUR_SECRET
```

## Tools

### Core

| Tool                     | Description                                                   |
| ------------------------ | ------------------------------------------------------------- |
| `shopify_graphql`        | Execute any raw GraphQL query or mutation                     |
| `shopify_schema_search`  | Search the live schema by keyword (types, queries, mutations) |
| `shopify_schema_details` | Get full details for a specific type, query, or mutation      |

### Products

| Tool                     | Description                                    |
| ------------------------ | ---------------------------------------------- |
| `shopify_products_list`  | List/search products with pagination           |
| `shopify_product_get`    | Get product by ID with variants and metafields |
| `shopify_product_create` | Create a new product                           |
| `shopify_product_update` | Update an existing product                     |
| `shopify_product_delete` | Delete a product                               |

### Collections

| Tool                        | Description                             |
| --------------------------- | --------------------------------------- |
| `shopify_collections_list`  | List/search collections with pagination |
| `shopify_collection_get`    | Get collection by ID with products      |
| `shopify_collection_create` | Create a collection (manual or smart)   |
| `shopify_collection_update` | Update a collection                     |
| `shopify_collection_delete` | Delete a collection                     |

### Metaobjects

| Tool                                  | Description                                |
| ------------------------------------- | ------------------------------------------ |
| `shopify_metaobject_definitions_list` | List all metaobject type definitions       |
| `shopify_metaobjects_list`            | List entries of a specific metaobject type |
| `shopify_metaobject_get`              | Get a single metaobject entry              |
| `shopify_metaobject_create`           | Create a new metaobject entry              |
| `shopify_metaobject_update`           | Update an existing metaobject entry        |
| `shopify_metaobject_delete`           | Delete a metaobject entry                  |

### Metafields

| Tool                       | Description                             |
| -------------------------- | --------------------------------------- |
| `shopify_metafields_list`  | List metafields on any resource         |
| `shopify_metafields_set`   | Set (upsert) metafields on any resource |
| `shopify_metafield_delete` | Delete a metafield                      |

### Customers

| Tool                      | Description                           |
| ------------------------- | ------------------------------------- |
| `shopify_customers_list`  | List/search customers with pagination |
| `shopify_customer_get`    | Get customer by ID with orders        |
| `shopify_customer_update` | Update customer details               |

### Orders

| Tool                  | Description                        |
| --------------------- | ---------------------------------- |
| `shopify_orders_list` | List/search orders with pagination |
| `shopify_order_get`   | Get order by ID with line items    |

### Inventory

| Tool                           | Description                             |
| ------------------------------ | --------------------------------------- |
| `shopify_inventory_get_levels` | Get inventory levels across locations   |
| `shopify_inventory_adjust`     | Adjust inventory quantity at a location |

## Required API Scopes

Configure these scopes on your app to enable all tools:

| Scope                                   | Tools                 |
| --------------------------------------- | --------------------- |
| `read_products`, `write_products`       | Products, inventory   |
| `read_metaobjects`, `write_metaobjects` | Metaobjects           |
| `read_content`, `write_content`         | Metafields            |
| `read_customers`, `write_customers`     | Customers             |
| `read_orders`                           | Orders                |
| `read_inventory`, `write_inventory`     | Inventory adjustments |
| `read_product_listings`                 | Product listings      |

You only need scopes for the tools you plan to use. At minimum for schema introspection and raw GraphQL to work, you need at least one read scope.

## Authentication

### OAuth Client Credentials (recommended)

For apps created in the [Shopify Dev Dashboard](https://partners.shopify.com):

1. Create an app in the Dev Dashboard
2. Configure the Admin API scopes listed above
3. Release a version and install the app on your store
4. Use the Client ID and Client Secret:

```bash
npx shopify-graphql-admin-mcp \
  --store mystore.myshopify.com \
  --client-id YOUR_CLIENT_ID \
  --client-secret YOUR_CLIENT_SECRET
```

### Legacy Access Token

For existing custom apps with a `shpat_` token:

```bash
npx shopify-graphql-admin-mcp \
  --store mystore.myshopify.com \
  --access-token shpat_xxxxx
```

## Configuration

| Flag              | Env Variable            | Description                                                           |
| ----------------- | ----------------------- | --------------------------------------------------------------------- |
| `--store`         | `SHOPIFY_STORE`         | Store domain (required). Accepts `mystore` or `mystore.myshopify.com` |
| `--access-token`  | `SHOPIFY_ACCESS_TOKEN`  | Legacy access token (`shpat_...`)                                     |
| `--client-id`     | `SHOPIFY_CLIENT_ID`     | OAuth client ID                                                       |
| `--client-secret` | `SHOPIFY_CLIENT_SECRET` | OAuth client secret                                                   |
| `--api-version`   | `SHOPIFY_API_VERSION`   | API version (default: `2026-07`)                                      |

## Usage with Claude Code

```bash
claude mcp add shopify -- npx shopify-graphql-admin-mcp \
  --store mystore.myshopify.com \
  --access-token shpat_xxxxx
```

Or add to your project's `.claude.json`:

```json
{
  "mcpServers": {
    "shopify": {
      "command": "npx",
      "args": [
        "shopify-graphql-admin-mcp",
        "--store",
        "mystore.myshopify.com",
        "--access-token",
        "shpat_xxxxx"
      ]
    }
  }
}
```

## Usage with Claude Desktop

Add to your Claude Desktop config (`~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "shopify": {
      "command": "npx",
      "args": [
        "shopify-graphql-admin-mcp",
        "--store",
        "mystore.myshopify.com",
        "--access-token",
        "shpat_xxxxx"
      ]
    }
  }
}
```

## Schema Exploration

The server introspects Shopify's live GraphQL schema when it starts. Your AI assistant can then discover queries, mutations, and types for the selected API version.

```
You: "What mutations are available for metaobjects?"
→ AI uses shopify_schema_search with query "metaobject" filter "mutations"
→ Returns: metaobjectCreate, metaobjectUpdate, metaobjectDelete, metaobjectUpsert, ...

You: "What fields does MetaobjectCreateInput take?"
→ AI uses shopify_schema_details with name "MetaobjectCreateInput"
→ Returns: full type definition with all fields, types, and descriptions
```

When Shopify releases a new API version, the server reads its schema at startup. The code does not need a schema update.

## Development

```bash
git clone https://github.com/colbymchenry/shopify-graphql-admin-mcp.git
cd shopify-graphql-admin-mcp
pnpm install
pnpm run build
pnpm run dev  # watch mode
```

## License

MIT
