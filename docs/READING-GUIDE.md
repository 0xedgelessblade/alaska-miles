# Public preview reading guide

## Start here

- Human interface and primary source: https://miles.edgelessblade.com/
- HTML summary: https://miles.edgelessblade.com/availability
- JSON summary: https://miles.edgelessblade.com/availability.json
- Discovery: https://miles.edgelessblade.com/sitemap.xml
- Crawler directives: https://miles.edgelessblade.com/robots.txt

No account or JavaScript is required for the summary. Both formats expose the same guest preview: at most three distinct departure dates per route and cabin within the public 60-day window. A missing date is not proof that no seat exists outside this preview.

## Fields and freshness

Use `publishedAt`, `snapshotDate`, and `datasetVersion` from the live response. They describe the published source, not a future update schedule. `window` defines the public date range. Read `departureDate`, `departureTime`, and `departureTimezone` together. `null` means unknown; an empty list means no matching public preview.

`seatsAtLeast` marks a lower bound. Points and taxes are per person, one way; retain the supplied currency. These are snapshots, not live booking guarantees.

## Efficient requests

Use GET or HEAD. Save the ETag and send it as If-None-Match on later requests. A 304 response means the saved body can be reused. Honor Retry-After for 429 or 503 responses; do not retry in parallel. If the header is absent, use increasing delays with random jitter and stop after a small bounded number of retries.

Do not try to expand the window with parameters, enumerate private endpoints, or bypass sign-in. The preview is an entry point, not a full inventory export.

## Citation and links

Name the source **Alaska 里程神器, by edgelessblade（重劍無鋒）**. Link to the website or the supplied routeUrl for the referenced route, and retain the publication time. For a machine-readable excerpt, also link to the public summary.

Suggested wording: “A public preview from Alaska 里程神器 lists these dates. Availability may have changed; check the airline before booking.”

## 分享給朋友

可直接分享地圖與公開摘要，不需要先登入。請保留來源連結與資料時間；不要把預覽或截圖當成即時訂位保證。

For product changes, see [release notes](../CHANGELOG.md). For the creator, see [edgelessblade](https://edgelessblade.com/) and [Threads](https://www.threads.com/@edgeless.blade).
