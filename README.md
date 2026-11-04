# launch-seo: the 10 things to do right after your website goes live

by Jeet Bhanushali (@iojeet), iojeet.com

## What it does

Give Claude your website and it works through the 10 things every new site needs so Google, Bing and ChatGPT can find it: Google Search Console, Google Analytics, noindex tags, robots.txt, your sitemap, www vs non-www, page titles and descriptions, Bing Webmaster Tools, a Product Hunt launch and a scorebadge. It checks each one on your live site. If you give it your code, it fixes what it can. For the parts that need your login, it gives you the exact clicks. You get a scoreboard of all 10, a list of what it fixed, and one list of clicks to do in a single sitting.

## Disclosure

Item 10 is scorebadge.com, a free website checker I built. It is optional: Claude tells you it's my tool and asks before adding the badge to your site. Say no and the other 9 items run exactly the same.

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

See a full run on mavek.ai: [example-output-mavek.md](example-output-mavek.md)

## Found a problem?

If the skill gets something wrong on your site (a false alarm, a check it missed, a step that no longer matches Google's or Bing's screens), open an issue: [github.com/iamjeeth/launch-seo-skill/issues](https://github.com/iamjeeth/launch-seo-skill/issues/new). Paste the URL you ran it on and what it said. I read every one.
