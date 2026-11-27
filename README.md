# Tracker Lens

Upload a HAR file from your browser and see how many outside companies each page you visited
talked to. Compare two or more browsing sessions side by side.

**One metric:** the number of distinct third party domains contacted per page visit. A third party
domain is any registrable domain that differs from the page you visited, for example
`doubleclick.net` on a news site.

Everything runs in your browser. Files are never uploaded anywhere.

## Use it

Live version: https://novasyntax811.github.io/tracker-lens/

Or download `index.html` and open it in Chrome or Firefox (double click is enough).

1. Open Chrome DevTools with F12 and go to the **Network** tab. Tick **Preserve log**.
2. Clear the log, then visit the pages you want to study.
3. Right click in the request list and choose **Save all as HAR**.
4. Drop the file onto the page. Repeat with a second browsing profile to compare.

No HAR file at hand? Click **Load example files**, or drop the two files from `examples/`.

## What you get

- **What the files show:** the results in plain sentences. The first file is the baseline; any
  other file is compared with it (use **Make baseline** to switch). Websites found in only one file
  are recognised as noise, their share of all visits is reported, and they are left out of the
  comparison.
- **Outside domains per page:** the average per file, plus recording time, browser, original file
  name and the websites in visit order, so files are easy to tell apart.
- **Site by site:** websites found in every file are compared, with the change per website. Noise
  websites are listed separately below.
- **What changed on the compared websites:** outside domains that are new or gone on those
  websites. Domains that only the noise websites contacted are kept apart.
- **Every page:** each page in visit order with its outside domains and request counts. Noise
  websites are labelled.
- **CSV export** with one row per page:
  `file_label, is_baseline, page_url, site, compared, third_party_domains, third_party_requests`

## How the metric is computed

1. Requests are grouped into pages using the HAR `pageref` field. Files without it are treated
   as one page.
2. Each request URL is reduced to its registrable domain: `ads.g.doubleclick.net` becomes
   `doubleclick.net`. Two part endings such as `co.uk` are handled by a short list
   (`TWO_PART_SUFFIXES` at the top of the script). It is not the full public suffix list, so a few
   rare endings may be grouped imperfectly.
3. A request is third party if its registrable domain differs from the page's.
4. Per page, the tool counts distinct third party domains and third party requests.

## Limits

- It measures what your browser contacted, not what those companies did with the data.
- Ad blockers, consent choices, time of day and login state all change the result. Keep them the
  same across the sessions you compare.
- Chrome removes cookies from exported HAR files by default. That does not affect this metric.

## Adding a second metric

All counting happens in `analyseHar()`. Add a new field to each page object there, then show it in
`renderCards()`, `renderPages()` and `downloadCsv()`. The existing metric stays untouched.

## License

MIT
