# Chinabank Transactions Scraper

A browser bookmarklet that copies your Chinabank unbilled credit card transactions to clipboard as a tab-separated table — paste directly into Google Sheets or Excel.

---

## Installation

1. Right-click your bookmarks bar → **Add page**
2. Set any name, e.g. `📋 Copy Transactions`
3. Paste the following into the **URL** field:

```
javascript: (() => {  const data = [["Date", "Description", "Amount"]];  document.querySelectorAll(".bb-list__item").forEach((group) => {    const dateEl = group.querySelector(".bb-transaction-group__date");    const date = dateEl ? dateEl.textContent.trim() : "";    group.querySelectorAll(".bb-transaction-header").forEach((row) => {      const desc =        row          .querySelector(".bb-transaction-item-description__title")          ?.textContent.trim() ?? "";      const integer = row.querySelector(".integer")?.textContent.trim() ?? "";      const decimal = row.querySelector(".decimals")?.textContent.trim() ?? "";      const amount = integer && decimal ? `${integer}.${decimal}` : "";      if (desc) data.push([date, desc, amount]);    });  });  const tsv = data.map((r) => r.map((c) => `"${c}"`).join("\t")).join("\n");  navigator.clipboard    .writeText(tsv)    .then(() =>      alert(`✓ Copied ${data.length - 1} transactions — paste into Sheets`),    );})();
```

4. Click **Save**

---

## Usage

1. Log in to [My CBC Online](https://digital.chinabank.ph/web/en/)
2. Navigate to **Credit Card → Transactions**
3. Scroll down to load all transactions you want to capture
4. Click the bookmark in your bookmarks bar
5. A confirmation alert tells you how many rows were copied
6. Paste (`Ctrl+V`) into Google Sheets or Excel

Output columns: **Date · Description · Amount**

---

## Compatibility

- **Browser:** Chrome or any Chromium-based browser (Edge, Brave, Arc)
- **Portal:** MyCBC Online (`https://digital.chinabank.ph/web/en/`)
- Built against the transaction list component structure as of March 2026. If ChinaBank updates their frontend the bookmarklet may stop working.

---

## If It Breaks

The selectors to check in the script:

| What it targets | Selector |
|----------------|----------|
| Date group header | `.bb-transaction-group__date` |
| Each transaction row | `.bb-transaction-header` |
| Merchant name | `.bb-transaction-item-description__title` |
| Amount (whole number) | `.integer` |
| Amount (decimals) | `.decimals` |

---

## Disclaimer

This bookmarklet reads data already visible on your screen in your own logged-in session. It does not make any API calls, store any data, or transmit anything anywhere. All processing happens locally in your browser.

---

*Made by [Gino Araullo](https://gino.media)*
