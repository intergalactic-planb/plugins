# Hard sources: a PDF, a site, a text, a messy Excel, several files

The import takes only files: Excel (.xlsx, .xls), CSV (.csv) and XML (.xml, including existing YML feeds). Prepare one XML file for a PDF, any mapped price, or sections the user wants to keep. Other flat sources can use CSV. Many products for a new catalog always go through one file and one import, never by create_catalog, create_field and create_products one by one.

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
- Ask which currency to use for the catalog if the choice is unclear. If the requested currency is refused, list the available choices from the refusal and wait for the user to pick one. Call import_to_new_catalog again, show the revised table with its new summary and wait for a separate yes to it before commit_import. Keep every amount and its supported source currency. For an unsupported price column currency, follow references/columns.md before preparing the preview.
- The characteristics that most products have are columns of the kind text (a size, a material, a weight…); the ones of one product go into the description.
- Pictures: a site gives them — a column of the kind list with the full addresses of the pictures of the product (https://…, as the page shows them, not made up); several in one cell are separated by ; (put such a cell in double quotes). A PDF gives none.
- Never add a column that no source has.

## Prepared XML for prices

Use UTF-8 and put the XML declaration at the start. Use the chosen catalog currency in the import request and the currency of each money column on its own field. The example has an EUR catalog with an EUR main price and a separate CHF column.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<export>
  <name>Supplier catalog</name>
  <categories>
    <category id="1">Gates</category>
    <category id="2" parentId="1">Sliding</category>
  </categories>
  <fields>
    <field id="col-name" name="Name" type="string"/>
    <field id="col-sku" name="SKU" type="string"/>
    <field id="col-price" name="Price" type="money" currencyId="EUR"/>
    <field id="col-wholesale" name="Wholesale" type="money" currencyId="CHF"/>
  </fields>
  <tags>
    <tag id="col-name">name</tag>
    <tag id="col-sku">vendorCode</tag>
    <tag id="col-price">price</tag>
  </tags>
  <offers>
    <offer id="G-100">
      <categoryId>2</categoryId>
      <param name="col-name">Sliding gate 4 m</param>
      <param name="col-sku">G-100</param>
      <param name="col-price">1250.00</param>
      <param name="col-wholesale">125.50</param>
    </offer>
  </offers>
</export>
```

- Put the catalog name first. For an update use the current catalog name and omit `<tags>` so existing roles are kept.
- Define every mapped column before the offers. Use a distinct ID starting with `col-` followed by lowercase letters, digits or hyphens. Use the ID as the source in the mapping. Types in the XML are string, money, number and list. For a new catalog, add tags for the agreed roles of the name, main price, code and image fields when present.
- Write currency codes only on the money field definitions. Write only decimal amounts in the price params. Omit each missing price param.
- For pictures, use a list field and one param with the complete link per picture. A PDF gives no pictures. Escape XML characters in names and values.
- A flat file omits the categories and category IDs. With sections, use numeric category IDs and reference only listed categories in the offers.

## Section changes in an existing catalog

- Compare the full paths from the file with the catalog before preparing XML. Use exact section names under the same parent, as in the snapshot (for example, "Gates / Sliding"). Check differences in spelling, spaces and case so they do not create a second section by mistake.
- Keep: existing products stay in their sections. Move: a product leaves all its previous sections and goes to its section from the file, or to the root if none is given. Missing sections are created. Add: products keep their previous sections and can gain a section that already exists.
- In add mode the import skips assignments to missing sections and applies the other changes. Show the missing full paths and affected products in the preview summary. Count each product gaining an existing section once. If the user explicitly asks to create missing sections, do so and prepare a fresh preview.
- Before asking for approval, show the affected products, where they are going, how many change sections, which paths would be created and which products would move to the root. State which memberships are lost or kept. The preview needs the count and the missing paths for a move or add.

## Then

1. From then on work only with the XML or CSV you made, do not read the source again.
2. Show the user the rows (all of them up to 30, else the first ones) with their sections, what is missing (no price…) and the currency (of the catalog, and of the workspace when it is another one). If the user corrects something, show the rows again.
3. A new catalog from PDF: import_to_new_catalog with the name of the source and .xml as fileName (supplier-catalog.xml) and the whole XML in fileText. It changes nothing and gives a summary and a code. Show the summary with the rows and wait for a yes; then commit_import with the code starts the import at once.
4. An update from PDF: match the rows with the catalog first, then make XML for the import with the key of the catalog and only the values that change; give it in fileText of preview_import.

## A link from the user

- A link to a file: download and read it first. For price imports, send the prepared XML in fileText or by the upload command. Pass links directly only for files with no mapped price columns.
- A link to a page or a site is not a file: read the pages, make XML for prices or sections, or CSV for other flat data and give it in fileText.
- You can not download it (no network): ask the user to attach the file.
