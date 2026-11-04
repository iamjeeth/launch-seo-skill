# launch-seo on mavek.ai (4 Nov 2026)

URL only, no code access, so nothing was fixed in code. 137 pages from the sitemap checked.

## Scoreboard

| # | Item | Status | What I found |
|---|---|---|---|
| 1 | Search Console | done | No verification meta tag, but 3 google-site-verification TXT records on the domain (DNS verified) |
| 2 | Analytics | done | GA4 G-4HT4R5D0MC on all 137 pages, Microsoft Clarity too |
| 3 | noindex | done | 0 of 137 pages have noindex (meta or X-Robots-Tag header) |
| 4 | robots.txt | done | Everything allowed except /api/. Googlebot, Bingbot, GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, PerplexityBot all allowed. Sitemap line present |
| 5 | Sitemap | done | /sitemap.xml 301s to /sitemap-index.xml (fine). All 137 URLs return 200, canonicals match |
| 6 | www vs non-www | done (small fix) | Everything lands on https://mavek.ai/ with 301s, but http://www.mavek.ai takes 2 hops |
| 7 | Titles and descriptions | broken (low priority) | None missing or duplicated. 48 titles over 60 characters (mostly blog posts ending in " \| Mavek", longest 93). 10 descriptions over 160 (longest 256) |
| 8 | Bing Webmaster Tools | check | No msvalidate.01 tag. Normal if imported from Search Console. Confirm in bing.com/webmasters |
| 9 | Product Hunt | check | producthunt.com blocks automated fetches. Ask the owner. Homepage does not link to a Product Hunt page |
| 10 | scorebadge | needs you | 95/100 on 11 Oct (speed 86, SEO 92, security 100, AI readiness 100). Badge not on the site |

## Your clicks

1. scorebadge.com: re-run the check, then paste the badge into the footer.
2. bing.com/webmasters: confirm mavek.ai is there with the sitemap submitted.
3. Domain settings: send http://www.mavek.ai straight to https://mavek.ai/ in one 301.
4. Drop " | Mavek" from blog titles. Then trim the 10 long descriptions.
