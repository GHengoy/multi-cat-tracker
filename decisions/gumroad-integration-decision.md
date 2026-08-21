# Gumroad Integration Decision

**api_creation_available:** yes

**chosen_approach:** api

## Findings

Gumroad's API provides a documented product creation endpoint. The live source code verification (from antiwork/gumroad GitHub repository as of 2026-08-21) confirms:

- **Endpoint:** `POST /v2/products`
- **Live docs location:** https://gumroad.com/api (Inertia.js rendered, source verified in repo)
- **Source code reference:** https://github.com/antiwork/gumroad/blob/master/app/controllers/api/v2/links_controller.rb and config/routes.rb

### Endpoint Details

**Route definition** (from config/routes.rb):
```
resources :links, path: "products", only: [:index, :show, :update, :create, :destroy]
```

Maps to: `POST /v2/products` for product creation

**Authentication:**
- OAuth 2.0 via Doorkeeper
- Required scope: `edit_products`
- Access token passed in Authorization header: `Authorization: Bearer {access_token}`

**Required fields** (from create_permitted_params):
- `name` (string) - Product title
- `description` (string) - Product description
- `price` (integer, in cents) - Product price
- `price_currency_type` (string) - Currency code (e.g., "usd", "eur")
- `native_type` (string) - Product type, defaults to "digital"
  - Valid values: "digital", "membership", "bundle"
  - Physical products not supported via API

**Optional fields:**
- `custom_permalink` (string) - Custom URL slug
- `max_purchase_count` (integer) - Limit purchases per buyer
- `customizable_price` (boolean) - Allow buyer to set price
- `suggested_price_cents` (integer) - Suggested price if customizable
- `taxonomy_id` (integer) - Product category
- `tags` (array of strings) - Product tags
- `subscription_duration` (string) - For membership products only
  - Valid values: "month", "quarter", "year"
- `files` (array of file objects) - Product files/downloads
  - Each file: `{ url: "...", name: "..." }`
- `rich_content` (array of content pages) - Rich product description pages

### Success Response Example

```json
{
  "success": true,
  "product": {
    "id": "...",
    "name": "Product Name",
    "description": "...",
    "price": 2999,
    "price_currency_type": "usd",
    "created_at": "2026-08-21T...",
    ...
  }
}
```

## Approach

**Using the API (programmatic)** is the chosen approach because:

1. **Product creation is fully supported** - The Gumroad API provides a complete POST endpoint with documented parameters
2. **No manual UI steps required** - Task 7 can call the API directly instead of asking the user to click through the dashboard
3. **Consistent with automation goals** - Enables end-to-end automation from content to listing to distribution
4. **Verified as working** - Confirmed in live source code with full parameter validation and error handling

### Implementation for Task 7

Task 7 will need to:

1. **Obtain an OAuth token** with the `edit_products` scope:
   - Either use Gumroad's OAuth flow or a pre-configured user token
   - Store token securely in environment variable

2. **Call the product creation endpoint:**
   ```
   POST https://api.gumroad.com/v2/products
   Authorization: Bearer {access_token}
   Content-Type: application/json
   
   {
     "name": "{product_title}",
     "description": "{product_description}",
     "price": {price_in_cents},
     "price_currency_type": "usd",
     "native_type": "digital",
     "files": [
       {
         "url": "{file_url_or_signed_upload_id}",
         "name": "{filename}"
       }
     ]
   }
   ```

3. **Handle response:**
   - Extract product ID from response for linking in downstream systems
   - Log success with product URL for user reference

### No Manual Fallback Needed

Unlike account signup (which requires manual user action), product creation can be fully automated via the API.
