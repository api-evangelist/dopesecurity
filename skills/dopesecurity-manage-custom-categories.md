---
name: Curate custom URL categories
description: Create custom URL categories and manage their URL membership in the dope.security Flightdeck API.
api: openapi/dopesecurity-flightdeck-openapi.yml
operations:
  - generateAccessToken
  - listCustomCategories
  - createCustomCategory
  - listCustomCategoryUrls
  - addCustomCategoryUrls
  - deleteCustomCategoryUrl
---

# Curate custom URL categories

Custom categories group URLs so a policy can allow, block, warn, or ignore them.

## Steps

1. Get a bearer token (`generateAccessToken`).
2. List existing categories with **`listCustomCategories`** — `GET /custom_categories`
   (cursor-paginated) to avoid name collisions.
3. Create one with **`createCustomCategory`** — `POST /custom_categories/{custom_category_name}`.
4. Inspect its URLs with **`listCustomCategoryUrls`** — `GET /custom_categories/{custom_category_name}/urls`.
5. Add URLs with **`addCustomCategoryUrls`** — `POST /custom_categories/{custom_category_name}/urls`
   (appends). Use the `PUT` variant (`overwriteCustomCategoryUrls`) to replace the whole set.
6. Remove one URL with **`deleteCustomCategoryUrl`** —
   `DELETE /custom_categories/{custom_category_name}/url/{encoded_url}` (URL-encode the value).

## Rules

- `POST` create returns `400` if the category name already exists — list first or handle the conflict.
- Whole-category and wipe-all-URL deletes are destructive; in the MCP server they require the
  destructive tier. Prefer per-URL deletes for routine curation.
