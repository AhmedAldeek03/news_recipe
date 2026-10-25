# News

A Drupal recipe that adds a **News** content type to a Seeds site: Seeds fields, a tabbed edit
form, URL aliases, a listing page, a *Latest news* block and one sample article.

## Fields

Fields are named after their field type, never after their purpose, so other content types can
share them. The one exception is `field_page_url`, which is named for its job because Pathauto
depends on it.

| Field                  | Type                                 | Label         | Notes                                   |
|------------------------|--------------------------------------|---------------|-----------------------------------------|
| `title`                | Title                                | Title         | Length indicator (30–60 characters)     |
| `field_date`           | Date (date only)                     | Date          | Required, defaults to today             |
| `field_term`           | Taxonomy terms: Tags                 | Tags          | Tagify input, new tags can be typed in  |
| `field_body`           | Formatted text, long, with summary   | Body          | Required, **Basic editor** format only  |
| `field_page_url`       | Plain text                           | Page URL      | Required, builds the URL alias          |
| `field_real_time_seo`  | Real-time SEO (Yoast)                | Real-time SEO |                                         |
| `field_meta_tags`      | Meta tags                            | Meta tags     | In the sidebar                          |
| `field_media`          | Media: Image                         | Image         | Required                                |
| `field_media_1`        | Media: File (unlimited)              | Attachments   |                                         |

## Edit form

Follows the Seeds conventions: the title on top, then horizontal tabs.

- **Basic Information**: Date, Tags, Body (with summary), Page URL, Real-time SEO
- **Media**: Image and Attachments, each editable in place

Author, authored on, URL alias, Meta tags and Published stay in the sidebar.

## Display

| View mode   | Shows                                                        |
|-------------|--------------------------------------------------------------|
| **Default** | Image (wide), date, body, attachments, tags                  |
| **Teaser**  | Image (medium, linked), date, summary or trimmed body        |

## Listing

View `news`, sorted by Date (newest first):

| Display          | What it shows                                    |
|------------------|--------------------------------------------------|
| Page (`/news`)   | All published news, 9 per page with a pager      |
| Block            | *Latest news*: the 3 latest items and an *All news* link. Place it wherever you need it. |

## Also included

- **URL aliases** built from the Page URL field: `/news/[page-url]` (Pathauto).
- **Permissions** for the **Content editor** role: create, edit, delete and revert News, use the
  Basic editor format, add image and file media, and create tags.
- **Sample content**: one published article with an image and two tags, owned by the
  administrator. Edit or delete it like any other content.

## Input

The recipe asks for one value when it is applied:

| Input        | Default | Used for                                      |
|--------------|---------|-----------------------------------------------|
| `url_prefix` | `news`  | The listing page (`/news`) and article aliases (`/news/[page-url]`) |

## Requirements

- Drupal 11.1 or later, with the Seeds profile (or any site with the `image` and `file` media
  types and the `basic_editor` text format)
- Field Group, Length Indicator, Media Library Edit, Metatag, Pathauto, Tagify, Real-time SEO

The recipe also applies the core **Tags** and **Content editor role** recipes.

## Installation

Seeds projects (`sprintive/seeds-project`) don't install recipes into `recipes/` yet. Their
`composer/installers` 1.x doesn't know the `drupal-recipe` package type, so add it once to the
project's `composer.json`, under `extra`:

```json
"installer-types": ["npm-asset", "drupal-recipe"],
"installer-paths": {
    "recipes/{$name}": ["type:drupal-recipe"]
}
```

Keep the other `installer-paths` entries. Then:

```bash
composer config repositories.news_recipe '{"type":"vcs","url":"https://github.com/AhmedAldeek03/news_recipe","no-api":true}'
composer require drupal/news_recipe:1.0.x-dev
drush recipe ../recipes/news_recipe
drush cache:rebuild
```

To skip the prompt, pass the input: `drush recipe ../recipes/news_recipe --input=news_recipe.url_prefix=news`.
With DDEV, put `ddev` before each command.

## Notes

- Recipes are applied once and cannot be uninstalled. The content type, fields and view stay
  and can be changed like any other configuration.
- Applying the recipe again is safe: existing config is kept and the sample article is not
  duplicated.
