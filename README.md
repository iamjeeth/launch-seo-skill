# launch-seo: the 10 things to do right after your website goes live (+ week 1 + week 2 + the full SEO pass)

by Jeet Bhanushali (@iojeet), iojeet.com

## What it does

Give Claude your website and it works through the 10 things every new site needs so Google, Bing and ChatGPT can find it: Google Search Console, Google Analytics, noindex tags, robots.txt, your sitemap, www vs non-www, page titles and descriptions, Bing Webmaster Tools, a Product Hunt launch and a scorebadge. It checks each one on your live site. If you give it your code, it fixes what it can. For the parts that need your login, it gives you the exact clicks. You get a scoreboard of all 10, a list of what it fixed, and one list of clicks to do in a single sitting.

**Week 1 (new):** run it again a week after launch and it adds 10 more checks: server-side rendering (does a crawler see your content or a blank page), site: index coverage, Microsoft Clarity, schema, Open Graph tags, linking Search Console to GA4, 404s, your forms, G2 and Capterra (software only) and a scorebadge audit.

**Week 2 (new):** two weeks in, Google has real numbers. It walks you through 10 more: the page indexing report, page 2 (striking distance) keywords, Clarity recordings, your sitemap in Bing, Core Web Vitals, competitor pages, a scorebadge audit, crawl stats, AI citations and Google Alerts. Most of these live in your Search Console, so it tells you exactly which report to open and what to paste back.

**The full SEO pass (new):** the rest of the list people throw at Claude in one prompt. Ask for it and it adds 14 more: redirect chains, canonical tags, one H1 per page, FAQ schema, breadcrumbs, orphan pages, alt text, WebP images, layout shift, load time, filler content, author bios, backlinks and directories. It fixes what lives in your code. For backlinks, directories and page speed scores it tells you the truth: what it can't do for you, and the exact steps that are yours. It will not promise you a ranking by Friday.

## Disclosure

Item 10 is scorebadge.com, a free website checker I built. It is optional: Claude tells you it's my tool and asks before adding the badge to your site. Say no and the other 9 items run exactly the same. The week-1 and week-2 parts have the same optional scorebadge audit (W10, T7). The AI citations check (T9) has a free way and mentions Mavek, a product I work on, only if you ask for a tool.

## Install

**Claude.ai (web or desktop app)**
1. Download `launch-seo.zip`. Do not unzip it.
2. Go to Settings > Capabilities and make sure Code execution is on (skills need it).
3. In Settings > Capabilities > Skills, click Upload skill and pick `launch-seo.zip`.
4. Turn the skill on. Web search should also be on.

**Claude Code** (best: it can read headers and fix your code)
1. Unzip `launch-seo.zip`.
2. Put the folder at `~/.claude/skills/launch-seo/` so the file sits at `~/.claude/skills/launch-seo/SKILL.md`:

```bash
mkdir -p ~/.claude/skills && unzip launch-seo.zip -d ~/.claude/skills/
```

3. Open Claude Code in your website's project folder. The skill loads on its own when you ask.

## Example prompt

```
Use the launch-seo skill on https://www.yoursite.com
```

Site live for a week already? Say so:

```
My site went live a week ago. Run launch-seo on https://www.yoursite.com
```

Two weeks in?

```
My site went live 2 weeks ago. Run launch-seo on https://www.yoursite.com
```

Want everything in one go?

```
Google isn't showing my website. Run the full launch-seo pass on https://www.yoursite.com
```

See a full run on mavek.ai: [example-output-mavek.md](example-output-mavek.md)

## Found a problem?

If the skill gets something wrong on your site (a false alarm, a check it missed, a step that no longer matches Google's or Bing's screens), open an issue: [github.com/iamjeeth/launch-seo-skill/issues](https://github.com/iamjeeth/launch-seo-skill/issues/new). Paste the URL you ran it on and what it said. I read every one.
