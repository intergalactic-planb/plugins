# Hard sources: a PDF, a site, a text, a messy Excel, several files

The import takes only files: Excel (.xlsx, .xls), CSV (.csv) and XML (.xml, including existing YML feeds). A PDF always becomes one native XML `<export>`, both with and without sections. For a site, text, messy Excel or several files, make one CSV, or the same XML when sections must be kept. Many products for a new catalog always go through one file and one import, never by create_catalog, create_field and create_products one by one.

## Read the source

- Read it with your own tools: a PDF with your tool that reads files, as it is; a site in the browser, all its pages and sections. Do not look for, check or install programs for PDF.
- A PDF of up to 10 pages: read the whole file at once, without a range of pages: a range of pages needs a program that may be missing. A longer PDF: read it in ranges of up to 10 pages; if that fails, ask the user to paste the text or send an Excel file.
- A site whose products appear only after the page runs its scripts (the HTML has no products): open it in the browser and go through every category and product page. No browser: say so and ask for a browser, an export of the products or their text. Never guess addresses of the site.
- Take only the products. Leave out the header of the document, the titles of sections (they become sections), the totals, notes like "with VAT".
- An Excel file that the import would read wrong (titles of sections in the rows, merged cells, empty columns, several properties in one cell): read every sheet yourself and make one table.
- Several files with a common identifier (a price list and a list of descriptions): read every file, join them by the identifier, one row a product.

## One product at a time

- The name exactly as written, letter by letter: do not change mixed Latin and Cyrillic letters, spaces or dashes.
- The price is the number only, with a dot (12,50 → 12.50). Never make it up.
- Several currencies, or a source currency refused for the catalog: ask which allowed currency is the currency of the catalog. Keep every source price in its own currency; never drop, convert or relabel one. The XML form is below.
- The characteristics that most products have are columns of the kind text (a size, a material, a weight…); the ones of one product go into the description.
- Pictures: a site gives them — a column of the kind list with the full addresses of the pictures of the product (https://…, as the page shows them, not made up); several in one cell are separated by ; (put such a cell in double quotes). A PDF gives none.
- Never add a column that no source has.

## A PDF: always XML

A PDF always becomes one XML file with the declaration and an `<export>` root. After the required `<name>`, a flat PDF has only `<offers>`. A PDF with sections (categories, chapters, titles of groups) also has `<categories>` and the offers have `<categoryId>`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<export>
  <name>Supplier catalog</name>
  <categories>
    <category id="1">Gates</category>
    <category id="2" parentId="1">Sliding</category>
  </categories>
  <offers>
    <offer id="G-100">
      <vendorCode>G-100</vendorCode>
      <name>Sliding gate 4 m</name>
      <price>1250.00</price>
      <currencyId>EUR</currencyId>
      <param name="Price USD" unit="USD">95</param>
      <categoryId>2</categoryId>
      <param name="Power">400 W</param>
    </offer>
  </offers>
</export>
```

- `<name>` with the catalog name must be the first child of `<export>`. For an update, use the current catalog name.
- A flat PDF omits the whole `<categories>` block and every `<categoryId>`.
- An `<offer>` is a product: `<name>`, `<price>` (a number), `<currencyId>` (the currency of the catalog), `<categoryId>` (no tag — the root), `<vendorCode>` if the source has a code, `<description>`, and a `<param name="…">` for each characteristic. For a site, add one `<picture>` for each picture address; a PDF gives none.
- `<price>` with `<currencyId>` is the main price in the currency of the catalog. A source price outside that currency, including a source currency refused for the catalog, is its own money column under a temporary name, the name of its price and the currency: `<param name="Price USD" unit="USD">95</param>`. Add `Price USD → Price USD → money` to the mapping. After the import rename it to the name of its price (references/columns.md, the currency of a money column). If a product has no source price in the catalog currency, omit `<price>`. Omit the param only when that product has no such source price. Never put the number into the main price, drop it, convert it or relabel it.
- No price ("on request", empty): omit `<price>` completely. Never write an empty `<price></price>` or `<price/>`: the import turns it into price 0.
- The ids of categories are numbers; every `<categoryId>` must be a listed `<category>`.
- Write & as &amp; and < as &lt;.
- In the mapping the column is the tag (name, price, vendorCode, description, picture) or the name of the param.

## Section changes in an existing catalog

- Compare the full paths from the file with the catalog before preparing XML. Use exact section names under the same parent, as in the snapshot (for example, "Gates / Sliding"). Check differences in spelling, spaces and case so they do not create a second section by mistake.
- Keep: existing products stay in their sections. Move: a product leaves all its previous sections and goes to its section from the file, or to the root if none is given. Missing sections are created. Add: products keep their previous sections and can gain a section that already exists.
- In add mode the import skips assignments to missing sections and applies the other changes. Show the missing full paths and affected products in the preview summary. Count only memberships being added to existing sections. If the user explicitly asks to create missing sections, do so and prepare a fresh preview.
- Before asking for approval, show the affected products, where they are going, how many change sections, which paths would be created and which products would move to the root. State which memberships are lost or kept. The preview needs the count and the missing paths for a move or add.

## Then

1. From then on work only with the XML or CSV you made, do not read the source again.
2. Show the user the rows (all of them up to 30, else the first ones) with their sections, what is missing (no price…) and the currency (of the catalog, and of the workspace when it is another one). Wait for a yes; if the user corrects something, show the rows again.
3. A new catalog from PDF: import_to_new_catalog with the name of the source and .xml as fileName (supplier-catalog.xml) and the whole XML in fileText. The import starts at once.
4. An update from PDF: match the rows with the catalog first, then make XML for the import with the key of the catalog and only the values that change; give it in fileText of preview_import.

## A link from the user

- A link to a file: download it and read it as a file first (its columns, first rows), then give the link in fileUrl. The service downloads it itself and checks that it is a file of the kind you read.
- A link to a page or a site is not a file: read the pages, make a CSV (or XML for sections) and give it in fileText.
- You can not download it (no network): ask the user to attach the file.
