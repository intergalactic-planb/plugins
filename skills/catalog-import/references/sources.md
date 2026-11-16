# Hard sources: a PDF, a site, a text, a messy Excel, several files

The import takes only files: Excel (.xlsx, .xls), CSV (.csv) and XML (.xml: XML of Bitrix or a YML feed). For anything else, make a CSV yourself (a YML, when the source has sections) and import it. Do it so also when the user did not speak of a file: many products for a new catalog always go through this CSV and one import, never by create_catalog, create_field and create_products one by one.

## Read the source

- Read it with your own tools: a PDF with your tool that reads files, as it is; a site in the browser, all its pages and sections. Do not look for, check or install programs for PDF.
- Take only the products. Leave out the header of the document, the titles of sections (they become sections), the totals, notes like "with VAT".
- An Excel file that the import would read wrong (titles of sections in the rows, merged cells, empty columns, several properties in one cell): read every sheet yourself and make one table.
- Several files with a common identifier (a price list and a list of descriptions): read every file, join them by the identifier, one row a product.

## One row a product

- The name exactly as written, letter by letter: do not change mixed Latin and Cyrillic letters, spaces or dashes.
- The price is the number only, with a dot (12,50 → 12.50). No price ("on request", empty) — an empty cell. Never make it up.
- The characteristics that most products have are columns of the kind text (a size, a material, a weight…); the ones of one product go into the description.
- Pictures: a site gives them — a column of the kind list with the full addresses of the pictures of the product (https://…, as the page shows them, not made up); several in one cell are separated by ; (put such a cell in double quotes). A PDF gives none.
- Never add a column that no source has.

## Sections: a YML, not a CSV

A CSV makes no sections. When the source has sections (categories, chapters, titles of groups), make a YML:

```xml
<yml_catalog date="2026-11-16"><shop>
<currencies><currency id="EUR" rate="1"/></currencies>
<categories>
  <category id="1">Gates</category>
  <category id="2" parentId="1">Sliding</category>
</categories>
<offers>
  <offer id="G-100"><vendorCode>G-100</vendorCode><name>Sliding gate 4 m</name><price>1250.00</price><currencyId>EUR</currencyId><categoryId>2</categoryId><param name="Power">400 W</param></offer>
</offers>
</shop></yml_catalog>
```

- An `<offer>` is a product: `<name>`, `<price>` (a number; no price — leave the tag out, an empty `<price></price>` becomes 0), `<currencyId>` (the currency of the catalog), `<categoryId>` (no tag — the root), `<vendorCode>` if the source has a code, `<description>`, a `<param name="…">` for each characteristic, a `<picture>` for each address of a picture.
- The ids of categories are numbers; every `<categoryId>` must be a listed `<category>`.
- Write & as &amp; and < as &lt;.
- In the mapping the column is the tag (name, price, vendorCode, description, picture) or the name of the param.

## Then

1. From then on work only with this CSV or YML, do not read the source again.
2. Show the user the rows (all of them up to 30, else the first ones) with their sections, what is missing (no price…) and the currency (of the catalog, and of the workspace when it is another one). Wait for a yes; if the user corrects something, show the rows again.
3. A new catalog: import_to_new_catalog with the name of the source and .csv or .xml as fileName (supplier-catalog.csv) and the whole text in fileText. The import starts at once.
4. An update: match the rows with the catalog first, then make the CSV for the import by references/matching.md (step 6: the key of the catalog, only the columns that change) and give that one in fileText of preview_import.

## A link from the user

- A link to a file: download it and read it as a file first (its columns, first rows), then give the link in fileUrl. The service downloads it itself and checks that it is a file of the kind you read.
- A link to a page or a site is not a file: read the pages, make a CSV or a YML and give it in fileText.
- You can not download it (no network): ask the user to attach the file.
