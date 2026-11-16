---
name: launch-seo
description: Runs the 10 SEO things to do right after a website goes live, 10 checks for week 1, 10 for week 2, and the full SEO pass a vibe-coded site needs, so Google, Bing and ChatGPT can find it. Use when someone launched (or is about to launch) a website and asks "what do I do now", "launch checklist", "post-launch SEO", "week 1 checks", "2 weeks after launch", "Google isn't showing my website", "fix the SEO on my vibe-coded site", or says "run launch-seo". Covers Search Console, Analytics, noindex, robots.txt, sitemap, www vs non-www, titles and descriptions, Bing, Product Hunt, SSR, schema, OG tags, 404s, forms, the indexing report, page 2 keywords, Clarity, Core Web Vitals, competitor pages, crawl stats, AI citations, then redirect chains, canonicals, H1s, breadcrumbs, orphan pages, alt text, WebP, speed, backlinks and directories. Discloses the author's own tools (scorebadge, Mavek) as optional. Checks the live site, fixes the code when it has the project, and gives exact clicks for what needs the owner's accounts.
---

# Launch SEO: the 10 things to do right after your website goes live (+ week 1 + week 2 + the full SEO pass)

Four parts. **Part 1** (the 10 items below) is for launch day. **Part 2** (week 1) is for a site that has been live for about a week. **Part 3** (week 2) is for a site that has been live for about 2 weeks, when Google has real data to read. **Part 4** (the full SEO pass) is the rest of the list people throw at Claude in one breath: "render it server-side, fix the 404s, add canonical tags, alt text, WebP, get me backlinks, rank me number 1 by Friday". Ask how long the site has been live if the user did not say: under a week = part 1 only; about a week = part 1 as a quick recheck, then part 2; 2 weeks or more = parts 1 and 2 as a quick recheck, then part 3. Run part 4 when the user asks for the full pass, pastes a list like that one, or says Google is not showing their site.

You take one live website and work through 10 items, in this order. For each one you check the live site, fix it in the code if you have the project open, and give the owner exact clicks for anything that needs their login.

## Inputs

- **Required:** the website URL.
- **Optional:** the project code (Claude Code, or files the user uploads). With the code you fix things yourself. Without it you hand over the exact change to make.

If the user gave only a URL, start right away.

## Ground rules

- Only report what you actually fetched. Never guess what a page, header or file says. If a fetch fails, say which URL failed and mark the item **check**.
- Plain language. Say "Google can't see this page", not "the page is non-indexable".
- No em dashes or en dashes in the output.
- Never ask for passwords, API keys or tokens. Anything that needs the owner's account is a **needs you** step with exact clicks.
- Every item ends with one status:
  - **done**: it was already right.
  - **fixed**: you changed the code. Say which file.
  - **needs you**: the owner has to click something. Give the steps.
  - **broken**: it is wrong and you could not fix it. Say what to change.
  - **check**: you could not confirm it.
  - **skipped**: the user chose not to do it (only item 10, week-1 items W9 and W10 and week-2 items T7 and T9 can be skipped), or the item does not apply to this site (part 4 says when).

## How to fetch

- In Claude Code, use `curl`: `curl -sIL <url>` shows headers and every redirect hop, `curl -sL <url>` gets the HTML.
- In Claude.ai, use web fetch. It may not show headers or redirect hops: when it does not, mark that part **check** and give the user the one `curl` line to run.
- Get the page list from `/sitemap.xml` (and child sitemaps). No sitemap: use the homepage navigation and footer links. Check up to 50 pages; say if you stopped early.

## The 10 items

### 1. Set up Google Search Console
- Look for `<meta name="google-site-verification"` in the homepage HTML, and for a `google-site-verification=` TXT record on the domain (`dig +short TXT domain`, or a DNS lookup site in Claude.ai). Either one present = probably verified: mark **done** and ask the owner to confirm the property shows data. Several TXT records is fine (one per person or tool that verified).
- Missing: **needs you**. Steps: open search.google.com/search-console, Add property, pick **Domain** (covers www, non-www, http and https), add the TXT record it gives to the domain's DNS, click Verify. If they cannot edit DNS, use the **URL prefix** property with the HTML tag method, and if you have the code, add the tag to the site's `<head>` yourself (**fixed**).

### 2. Set up Google Analytics
- Look in the homepage HTML for `googletagmanager.com/gtag/js?id=G-` or `googletagmanager.com/gtm.js`. Some sites build the tag URL in a script, so also search for a bare `G-XXXXXXXXXX` ID. Note the ID you find.
- Found: **done**. Also check 2 more pages have the same ID (a tag only on the homepage is **broken**).
- Missing: **needs you** to create the property (analytics.google.com, Admin, Create property, Web stream, copy the `G-` ID). With the code and the ID, add the tag to the shared layout so every page gets it (**fixed**).
- Tell them to link Search Console to Analytics: Analytics Admin, Product links, Search Console links.

### 3. Check for noindex tags
For every page on the list:
- HTML: `<meta name="robots"` or `<meta name="googlebot"` containing `noindex`.
- Header: `X-Robots-Tag: noindex`.
Any hit on a page that should rank is **broken** (the most common launch mistake: a noindex left on from the staging site). With the code, remove it (**fixed**). List every page with a hit.

### 4. Check your robots.txt
Fetch `/robots.txt`.
- `Disallow: /` under `User-agent: *` or `User-agent: Googlebot` blocks the whole site: **broken**, fix first.
- Check these are not blocked: Googlebot, Bingbot, GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, PerplexityBot. List any that are.
- It should have a `Sitemap:` line with the full sitemap URL. Missing: add it.
- No robots.txt at all is fine for Google (it means "allowed"), but create a small one with the Sitemap line.

### 5. Submit your sitemap
- Fetch `/sitemap.xml`. Missing: **broken**. With the code, generate one from the site's routes (most frameworks have a plugin or a built-in route), **fixed**.
- Every URL in it should return 200 with no redirect, and none should have a noindex (item 3). List the ones that fail.
- Then **needs you**: Search Console, Sitemaps, paste `sitemap.xml`, Submit. Then URL inspection on the homepage, Request indexing.

### 6. Pick www or non-www, redirect the other
Fetch all four: `http://domain`, `http://www.domain`, `https://domain`, `https://www.domain`.
- Right: three of them 301 to the fourth, in one hop.
- **broken** if two versions both load with 200, if a redirect is 302, or if it takes more than one hop. Say which.
- Also check the `<link rel="canonical">` on the homepage points to the version that wins.
- The fix is usually in the host's domain settings (Vercel, Netlify, Cloudflare, Webflow, Framer, Shopify): give the exact setting for the host you detect from the headers. With the code and a redirect config file, fix it there.

### 7. Give every page its own title and description
For every page on the list, read `<title>` and `<meta name="description">`.
- Flag: missing, the same as another page, title over about 60 characters, description over about 160.
- Missing or duplicate = fix now. Too long = lower priority (Google cuts it off in results, the page still ranks). If most long titles end in " | Brand", the quick fix is dropping the brand suffix on blog posts.
- Write a new title and description for every flagged page, using only what the page actually says: what it is plus who it is for. With the code, put them in (**fixed**). Without it, give a table: page, current, new.

### 8. Add it to Bing Webmaster Tools
- Bing's index feeds ChatGPT search and Copilot, so this is not optional anymore.
- **needs you**: open bing.com/webmasters, sign in, pick **Import from Google Search Console** (it copies the site and the sitemap in one go, no new verification). No Search Console yet: add the site by URL and verify with the meta tag (with the code, add it yourself).
- Look for `<meta name="msvalidate.01"` to see if it is already verified.

### 9. Launch on Product Hunt
First ask if it has already launched there (producthunt.com often blocks automated fetches, so dont rely on checking). Already launched: **done**, and check the site links to its Product Hunt page. Not yet: write the launch kit, only from what the site says:
- Name.
- Tagline, 60 characters max: what it does, for whom.
- Description, 260 characters max.
- 3 to 5 topics.
- Maker's first comment, about 120 words: why it was built, who it is for, one thing to try first, a question for the community.
- Gallery shot list: 4 to 6 screens to capture, in order.
Then **needs you**: schedule it at producthunt.com (Launch, Submit). Tell them to launch at 12:01 am Pacific, the start of Product Hunt's day.

### 10. Add a scorebadge (optional, the skill author's own tool)
**Disclosure, say this to the user before this item:** scorebadge.com is made by Jeet Bhanushali, who wrote this skill. It is optional. Items 1 to 9 do not depend on it, and skipping it changes nothing else in the scoreboard. The check is free; the badge is a small image with a link back to scorebadge.com. Only add the badge to the site's code if the user says yes. If they say no or do not answer, mark this item **skipped** and move on.
- **needs you**: open scorebadge.com, enter the domain, run the free check (speed, SEO, security, AI readiness). Fix anything red that the items above did not already cover.
- Then add the badge to the footer. The snippet is:
  `<a href="https://scorebadge.com/site/DOMAIN"><img src="https://scorebadge.com/badge/DOMAIN.svg" alt="DOMAIN website score on scorebadge" height="22"></a>`
  Replace DOMAIN with the bare domain (no https, no www unless the site uses it). With the code, add it to the shared footer (**fixed**), only after the user said yes. It updates every time they re-check.

## Part 2: the 10 things to do 1 week after your website goes live

Same ground rules, same statuses. By now Google has had a week to crawl, so this part checks what it actually picked up and what the first visitors hit.

### W1. Check your SSR (server-side rendering)
- Fetch the homepage and 2 inner pages WITHOUT running JavaScript (`curl -sL`). Look for the page's real text: the `<h1>`, a paragraph of body copy, the nav links.
- Only an empty shell like `<div id="root"></div>` or `<div id="__next"></div>` plus script tags = **broken**: crawlers that do not run JavaScript (most AI crawlers) see a blank page, and Google sees it late.
- With the code: switch those routes to server-side rendering or static generation (Next.js server components or `generateStaticParams`, Astro, Nuxt `ssr: true`, Vite + prerender plugin, etc.) and say which (**fixed**).
- **needs you** to confirm in Google: Search Console, URL Inspection, paste the URL, Test live URL, View tested page, HTML tab. The text must be there.

### W2. Run a site: search for index coverage
- Google blocks automated searches, so this is **needs you**: search `site:yourdomain.com` on Google and note the number of results.
- Compare it with the number of URLs in the sitemap (you have it from part 1). Far fewer = pages not indexed yet.
- Give the list of sitemap URLs to spot-check with URL Inspection; for any "URL is not on Google", click Request indexing (the main pages first, a handful a day).
- Also: Search Console, Pages report. List the reasons it shows for "not indexed" if the user pastes them.

### W3. Install Microsoft Clarity
- Look for `clarity.ms/tag` in the homepage HTML. Found: **done**.
- Missing: **needs you**: clarity.microsoft.com, New project, copy the tracking code (or connect it through Google Tag Manager or the GA4 integration). With the code and the project ID, add the snippet to the shared `<head>` (**fixed**). It is free: heatmaps and session recordings show where the first visitors click and where they leave.

### W4. Validate your schema
- Read every `<script type="application/ld+json">` on the homepage and 2 inner pages. Each must parse as JSON.
- Homepage: an `Organization` (or `LocalBusiness`) with name, url, logo and `sameAs` social links. Inner pages: `BreadcrumbList`. Blog posts: `Article` with headline, author and date. Missing ones = **broken**; with the code, add them using only facts on the page (**fixed**).
- **needs you**: search.google.com/test/rich-results, paste the URL, it should say "valid items detected".

### W5. Test your OG tags
- On the homepage and 2 inner pages, check `og:title`, `og:description`, `og:image`, `og:url` and `twitter:card`.
- `og:image` must be an absolute https URL that loads (fetch it: 200 and an image content type), ideally 1200x630. Every page sharing the same image is fine; a missing image is **broken** (links shared on LinkedIn, WhatsApp and X show up blank).
- With the code, add the tags to the shared layout and per page (**fixed**).
- **needs you**: linkedin.com/post-inspector, paste the URL, check the preview.

### W6. Link GSC to GA4
- **needs you**: Google Analytics, Admin, Product links, Search Console links, Link, pick the property and the web stream. Then the Search Console reports show up inside GA4 (Reports, Search Console), so search queries and landing page behaviour sit in one place.
- Already linked (the user can see it there): **done**.

### W7. Check your 404s
- Fetch a made-up URL like `/this-page-does-not-exist-123`. It must return status **404** (not 200, not a redirect to the homepage). A 200 = soft 404 = **broken**; with the code, make the catch-all route return a real 404 (**fixed**).
- It should show a custom 404 page with links back to the homepage and main pages.
- Fetch every internal link on the pages you checked; list any that return 404 and the page that links to them.
- **needs you**: Search Console, Pages, "Not found (404)". Redirect (301) each real old URL to its closest live page; leave true junk URLs as 404.

### W8. Test your forms
- Find every `<form>` (contact, lead, newsletter, booking embed) and note where it posts (action URL, form service, script handler).
- Never submit a real form yourself (it could email a real person or create a fake lead). With the code, check the handler actually sends: env variables set, the email or webhook target filled in, no leftover test endpoint.
- **needs you**: submit each form once with "TEST" in the message, confirm it arrives where they expect (inbox, CRM, Slack), and check spam. Also check the thank-you message or page shows.

### W9. Get on G2 and Capterra (software products only)
- Ask if the product is software sold to businesses. No: **skipped**.
- Yes: these review sites are some of the places ChatGPT and Google read about software. **needs you**: create or claim the free profile on G2 (search "G2 get listed") and on Capterra (search "list your software on Capterra"; Capterra is part of Gartner Digital Markets, one listing also covers GetApp and Software Advice). Write the listing text for them from what the site says: one-line description, 3 to 5 features, categories, pricing model.
- Then ask the first 3 to 5 happy customers for a review.

### W10. Run a scorebadge audit (optional, the skill author's own tool)
**Disclosure, say this to the user before this item:** scorebadge.com is made by Jeet Bhanushali, who wrote this skill. It is optional and nothing else in this part depends on it. If they say no or do not answer, mark it **skipped**.
- **needs you**: open scorebadge.com, enter the domain, run the free check (speed, SEO, security, AI readiness).
- List anything red that W1 to W9 did not already cover, with the fix. If they added the badge in part 1, it refreshes with this check.

## Part 3: the 10 things to do 2 weeks after your website goes live

Same ground rules, same statuses. After 2 weeks Google has crawled the site and Search Console has its first real numbers. Most of this part reads those numbers, and only the owner can open them, so ask the owner to paste or export each report. Never guess what a report says.

### T1. Check your page indexing report
- **needs you**: Search Console, Indexing, Pages. Paste the two totals (Indexed, Not indexed) and the list under "Why pages aren't indexed" with the count for each reason.
- Then go reason by reason and say what to do, in plain words:
  - "Excluded by noindex tag": item 3. Remove the tag on pages that should rank.
  - "Blocked by robots.txt": item 4.
  - "Page with redirect" and "Alternate page with proper canonical tag": usually fine. Check the listed URLs are the ones you expect.
  - "Not found (404)": W7.
  - "Crawled, currently not indexed" and "Discovered, currently not indexed": Google found the page and has not kept it or has not fetched it yet. Check the page has real content of its own (F11), that other pages link to it (F6), and that it is in the sitemap. Then URL Inspection, Request indexing, the important pages first.
- Compare Indexed with the number of URLs in the sitemap. With the code, fix what the reasons point to (**fixed**).

### T2. Find your striking distance keywords
- These are searches where the site already shows on page 2 (positions 11 to 20). They are the cheapest wins, because Google already thinks the page fits.
- **needs you**: Search Console, Performance, Search results, tick Average position, last 28 days. Add the filter Position greater than 10, sort by Impressions, Export. Share the file or paste the top rows (query, page, impressions, position).
- For the 10 queries with the most impressions at position 11 to 20: name the page that ranks, then say what that page is missing for that search: the words in the title and H1, a section that answers it, a link from a stronger page. With the code, make those edits (**fixed**).
- A 2-week-old site may have very few rows. Say so and say when to look again.

### T3. Send your Clarity recordings to Claude
- Needs Microsoft Clarity (W3). No Clarity yet: **needs you** to install it, then come back in a week.
- **needs you**: clarity.microsoft.com, Recordings. Watch 10 sessions, or open Dashboard and note the numbers for dead clicks, rage clicks, quick backs and excessive scrolling, and the pages they happen on. Paste what you see, or export and share.
- From that, name the 3 biggest leaks: a button people click that does nothing, a form people start and leave, a page people leave in seconds. Say the fix for each. With the code, make it (**fixed**).
- Never say what the recordings show unless the owner shared it.

### T4. Submit your sitemap to Bing
- Item 8 adds the site to Bing Webmaster Tools. This checks the sitemap actually went in.
- **needs you**: bing.com/webmasters, Sitemaps. The sitemap URL should be listed with a status of Success and a count of URLs. Not there: Submit sitemap, paste the full sitemap URL.
- Also use URL Submission for the homepage and the main pages. Bing's index feeds ChatGPT search and Copilot.

### T5. Check your Core Web Vitals
- **needs you**: Search Console, Experience, Core Web Vitals. A new site often shows "not enough data". Then use pagespeed.web.dev on the homepage and the 2 most important pages, on mobile, and paste LCP, INP and CLS.
- Google's "good" lines: LCP 2.5 seconds or less, INP 200 milliseconds or less, CLS 0.1 or less.
- Anything over: the fixes are in part 4, F8 (images), F9 (layout shift) and F10 (load time). With the code, do them (**fixed**).

### T6. Steal your competitor's keywords
- Ask for 2 or 3 competitors, by name or URL. Fetch each one's sitemap and main navigation and list the page types they have that this site does not: comparison pages, alternatives pages, use-case pages, pricing, integrations, a glossary, city pages.
- If the owner has an Ahrefs, Semrush or similar export of a competitor's keywords, read it and pick the searches this site has no page for. Without an export, say plainly that you can see the competitor's pages but not their search numbers, and never invent volumes.
- Output: a short list of pages to build, each with the search it is for and the competitor page that proves the demand. With the code, create the page shells only if the owner asks.

### T7. Run a free site audit on scorebadge (optional, the skill author's own tool)
**Disclosure, say this to the user before this item:** scorebadge.com is made by Jeet Bhanushali, who wrote this skill. It is optional and nothing else in this part depends on it. If they say no or do not answer, mark it **skipped**.
- **needs you**: open scorebadge.com, enter the domain, run the free check. If W10 already ran, compare the two scores and list what changed.
- List anything red that the other items did not already cover, with the fix.

### T8. Check your crawl stats
- **needs you**: Search Console, Settings, Crawl stats, Open report. Paste: total crawl requests, average response time, the "By response" split (OK 200, 301, 404, 5xx) and Host status.
- What to look for: Host status with any problem in the last 90 days (server errors, robots.txt fetch, DNS) comes first. A big share of 404 or 301 responses means Google is spending its visits on dead or moved URLs: fix with W7 and F1. Average response time over about 600 milliseconds: F10.
- Very few requests on a 2-week-old site is normal. Say so.

### T9. Check your AI citations (optional: Mavek is the skill author's own product)
**Disclosure, say this to the user before this item:** Mavek (mavek.ai) is a product Jeet Bhanushali, who wrote this skill, works on. Using it is optional. The free way below works without it.
- The free way, **needs you**: open ChatGPT, Perplexity and Google's AI Mode. Ask the 5 questions a buyer would ask before choosing a business like this one ("best X for Y", "X vs Z", "how do I ..."). For each answer, note whether the site is named and which sites are linked as sources.
- Then read those source pages and say what they have that this site does not: a comparison table, a clear answer near the top, a page on that exact question.
- Only if the user wants a tool for this: tell them Mavek tracks these answers over time, repeat that it is the author's product, and let them choose. Otherwise mark the tool part **skipped**.
- AI answers change from day to day. Report what was seen and when, never "you rank number 1 in ChatGPT".

### T10. Set up Google Alerts for your brand name
- **needs you**: google.com/alerts. Create an alert for the brand name in quotes, one for the domain, and one for the founder's name if they are the face of the business. How often: once a day. Sources: Automatic. How many: All results.
- Why: every new mention is a chance to ask for a link back (F13) or to answer a review.
- A brand name that is a common word needs a second word in the alert ("Acme" software), or it fills with noise.

## Part 4: the full SEO pass (the vibe coder's SEO prompt)

Same ground rules, same statuses. This part exists because people ask for all of it in one prompt. Some of that list is already in parts 1 to 3, so do not redo it: point to the item and reuse its result.

| They ask for | Where it is |
|---|---|
| Render it server-side | W1 |
| Generate a sitemap, submit it to Search Console | 5 and 1 |
| Unblock Googlebot and AI bots in robots.txt | 4 |
| Delete the noindex tags | 3 |
| Fix the 404s | W7 |
| Write meta descriptions | 7 |
| Core Web Vitals | T5, then F9 and F10 for the fixes |
| Everything else on the list | F1 to F14 below |

Check the same page list as part 1 (up to 50 pages from the sitemap). Run the checks on the HTML you fetched without JavaScript, because that is what crawlers get.

### F1. Kill the redirect chains
- For every URL in the sitemap and every internal link you found, follow the redirects (`curl -sIL`). Count the hops.
- Right: 0 hops for sitemap URLs and internal links. One 301 is fine for old URLs people may still have bookmarked.
- **broken**: 2 or more hops, any 302 or 307 on a permanent move, a redirect loop, or an internal link that points at a URL that redirects.
- With the code: point each chain straight at the final URL in the redirect config, and update the internal links so they use the final URL (**fixed**). List every chain: start, each hop, end.

### F2. Add canonical tags
- Every page needs one `<link rel="canonical">` in the `<head>`: an absolute https URL, on the host that won in item 6, pointing at the page itself.
- **broken**: missing, more than one, a relative URL, pointing at the homepage from every page (a common template mistake that tells Google the whole site is one page), or pointing at a URL that redirects or 404s.
- Pages that are true copies (print views, filtered lists, UTM versions) should point at the main version.
- With the code: set it once in the shared layout from the page's own path (**fixed**).

### F3. One H1 per page
- Count the `<h1>` tags on every page.
- Right: exactly one, visible, saying what the page is about in the words a customer would search.
- **broken**: none, more than one, an H1 that is only the logo or the brand name on an inner page, or an H1 hidden with CSS. Never hide a heading to pass this check.
- Also flag headings that skip levels for styling (an `<h4>` straight after the `<h1>`). With the code, fix the tags and keep the look with CSS (**fixed**).

### F4. Add FAQ schema
- Only for pages that already show questions and answers to the visitor. Never add FAQ markup for questions that are not on the page, and never invent questions.
- Page has a visible FAQ but no `FAQPage` in its JSON-LD: with the code, add it, copying the questions and answers word for word (**fixed**).
- Page has no FAQ: **skipped** for that page. If it is a pricing, product or service page, suggest 3 to 5 real questions the owner hears from customers and let them write the answers.
- Tell the owner straight: Google now shows FAQ rich results for very few sites, so this is mostly for Bing and AI answers, which read the question and answer pairs.
- **needs you**: search.google.com/test/rich-results, paste the URL, check it reads the FAQ with no errors.

### F5. Add breadcrumbs
- Any page deeper than one level (`/blog/post`, `/products/shoes/red`) should show a breadcrumb trail a visitor can click, and carry `BreadcrumbList` JSON-LD that matches it (W4 checks the schema, this checks both).
- **broken**: no trail on deep pages, a trail with no schema, or schema whose names or URLs do not match the visible trail.
- A flat site (homepage plus a few top-level pages) does not need them: **skipped**.
- With the code: add one breadcrumb component to the shared layout for deep routes and generate the schema from the same data (**fixed**).

### F6. Link the orphan pages
- An orphan is a page that is in the sitemap but that no other page links to. Build the list: every sitemap URL, minus every URL that appears as an internal link on the pages you fetched (nav, footer and body).
- Say how many pages you crawled so the owner knows how sure the list is. If you checked 50 of 400 pages, call the result a sample.
- For each orphan, suggest 1 to 3 pages that should link to it and the words to link from. With the code, add those links (**fixed**).
- Pages that should not rank at all (thank-you pages, old campaign pages) should come out of the sitemap instead.

### F7. Alt text on every image
- List every `<img>` with no `alt` attribute. Images that carry meaning (product shots, screenshots, charts, team photos) need a short plain description of what is in them. Decorative images (dividers, background shapes) get an empty `alt=""`, which is correct, not missing.
- Flag alt text that is a file name ("IMG_2231.jpg"), a keyword list, or the same on every image.
- With the code: write the alt text from what the image shows and the page it sits on (**fixed**). Without the image itself, do not guess what is in it: list those as **check**.

### F8. Convert the images to WebP
- For each image over about 100 KB, note its format and size (`curl -sI` gives content-type and content-length).
- **broken**: big JPEG or PNG photos, images served far wider than they are shown, no `width` and `height` on the tag, and images below the fold with no `loading="lazy"`.
- With the code: convert to WebP (or AVIF), keeping the original as a fallback where the framework needs one. Use the framework's image component if it has one (Next.js `next/image`, Astro `<Image>`, Nuxt Image), because it also sizes and lazy-loads (**fixed**). Report the before and after sizes.
- SVG logos and icons stay as SVG.

### F9. Fix the layout shift
- Layout shift is the page jumping while it loads. Look for the causes in the HTML and CSS: images, video and embeds with no `width` and `height` (or CSS `aspect-ratio`), web fonts loaded with no `font-display`, banners and cookie bars injected above the content, ads or widgets with no reserved space.
- With the code: add the dimensions, reserve the space, set `font-display: swap` or `optional` (**fixed**).
- **needs you** for the real number: pagespeed.web.dev, paste the URL, read CLS. Google counts 0.1 or less as good. You cannot measure it from a fetch, so never state a CLS number you did not get from the owner.

### F10. Get it loading in under 2 seconds
- Measure what you can: `curl -o /dev/null -s -w '%{time_starttransfer} %{time_total}' <url>` gives the server response time and the HTML download time. A first byte over about 0.8 seconds is a server or hosting problem: say so.
- Then look for the usual weight: images (F8), render-blocking scripts in the `<head>`, big JavaScript bundles, fonts from several hosts, third-party tags that load before the content, no caching headers, no compression.
- With the code: defer or remove what blocks rendering, lazy-load below-the-fold parts, set cache headers, and say what each change does (**fixed**).
- **needs you** for the real number: pagespeed.web.dev, read LCP on mobile. Google counts 2.5 seconds or less as good. Report it as "your server answered in X seconds from here", never as a promised load time.

### F11. Remove the filler and fake AI content
- No tool can reliably tell AI text from human text, so do not claim to detect it. Flag what a reader would call filler:
  - placeholder text left in (lorem ipsum, "Your tagline here", "Feature one"),
  - numbers, awards, customer names or testimonials with no source on the page or the site,
  - pages that say the same thing as another page in other words,
  - paragraphs that could sit on any company's site ("In today's fast-paced digital world").
- For each one, quote the line and say what should replace it: a real number, a real customer, a specific detail of the product. Ask the owner for the fact. Never write a fake one.
- With the code: remove placeholders and merge duplicate pages with a 301 from the weaker one (**fixed**). Rewrites of real claims are **needs you**.

### F12. Add an author bio
- For blog posts and guides: the post should show the author's name, a one or two line bio saying why they know this topic, and a link to an author or about page. The `Article` schema's `author` should be a `Person` with a `name` and a `url`.
- **broken**: no author, "admin" or the company name as the author, or an author with no page anywhere on the site.
- With the code: add the bio block to the post template and the `Person` to the schema (**fixed**). The bio text itself is **needs you**: ask for the real person's role and experience. Never invent credentials.
- Sites with no articles: **skipped**.

### F13. Get backlinks
- Be honest first: no skill can produce backlinks, and nobody can promise one from Forbes. Never suggest buying links or link exchanges.
- What you can do: list the links this site can actually earn, from what it is. Pick the ones that fit:
  - the Product Hunt launch (item 9) and G2 or Capterra (W9),
  - the founder's and company's own profiles (LinkedIn company page, X, GitHub, Crunchbase), each linking to the site,
  - partners, tools it integrates with, customers and suppliers who already mention it,
  - one piece worth linking to: its own data, a free tool, a clear how-to nobody else has written.
- If the user has a backlink export or Search Console's Links report, read it and list who links now.
- **needs you**: give the list as steps, most likely first, with the page on the site each one should point to.

### F14. Get listed on directories
- Pick directories by what the business is, not a list of 200:
  - a business with a physical location or a service area: Google Business Profile and Bing Places first, then Apple Business Connect, then the 2 or 3 directories its own trade uses,
  - software: G2 and Capterra (W9), then Product Hunt, AlternativeTo and the marketplace of any platform it plugs into,
  - any company: LinkedIn company page and Crunchbase.
- Write the listing text once from what the site says: name, one-line description, categories, address and phone exactly as they appear on the site. The name, address and phone must match everywhere.
- **needs you**: the list, in order, with the sign-up page for each.
- Skip paid "submit to 500 directories" services. Say so if the user asks about them.

### "Rank me number 1 by Friday, make no mistakes"
If the user asks for a ranking by a date, say it plainly: nobody controls where Google ranks a page or when, and this skill does not either. What it does is remove every reason on this list for Google to ignore the site. Then give the scoreboard.

## Output

1. **Scoreboard**: one table, every item you ran (1 to 10, then W1 to W10 for a week-old site, T1 to T10 for a 2-week-old site, F1 to F14 if part 4 ran): #, item, status, one line on what you found.
2. **What I fixed**: file by file (skip if you had no code).
3. **Your clicks**: only the **needs you** steps, numbered, in order, so the owner can do them in one sitting.
4. **Still broken**: anything **broken** or **check**, with the exact change to make.

## Finish

Launch day (part 1 only): end with "Run this again in a week and I'll check Google has picked up your pages."
Week 1 or later (part 2 ran): end with "Run this again in a month: Search Console will have real queries by then."
Week 2 (part 3 ran): end with "Run this again in a month: by then page 2 keywords and Core Web Vitals will have enough data."
Full pass (part 4 ran): end with the count of items done, fixed, needs you and broken, then "Do the clicks list, then run this again in 2 weeks."
