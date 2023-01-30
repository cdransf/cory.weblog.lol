---
Date: 2023-1-29 17:00
Tags: fastmail, email, workflows
---

# Workflows: handling inbound email on Fastmail with regular expressions

I've been using Fastmail for years now and have explored a number of different approaches to handling mail. I've approached it by creating rules targeting lists of top level domains, I've gone with no rules at all and a heavy-handed approach to unsubscribing from messages (operating under the idea that *everything* warrants being seen and triaged) and I've even used HEY [^1]. 

For now, I've approached filtering my mail by applying regular expressions to reasonably broad categories of incoming mail[^2]. My thinking with this approach is that will scale better over the long term by applying heuristics to common phrases and patterns in incoming mail without the need to apply rules to senders on a per address or domain basis.

## Alias-specific rules

I have four aliases that I regularly provide to different services. One is for newsletters and routes them to [Readwise's Reader app](https://readwise.io/read), another routes directly to my saved articles in the same app, another routes different messages to my [Things](https://culturedcode.com/things/) inbox and a final one serves as the recovery email on my grandfather's accounts (in the event anything goes awry).

These work by checking that the `To/CC/BCC` matches the appropriate alias before filing them off to `Archive/Newsletters`, `Archive/Saves` or `Notifications`, respectively. These folders are configured to auto-purge their contents at regular intervals as they are typically consumed in the context of the application that they're forwarded to (and are only filed into folders for reference in the event something goes wrong in said applications).

## A quick failsafe

In the event I've failed to tune a regular expression properly or an actual person triggers a match I have a rule that is executed after the aforementioned alias-specific rules that stops all rule evaluations for *any* address in my contacts.

[I also have a rule containing regular expressions that also skips evaluations for login pin codes, meeting/appointment reminders and common security notices](https://cory.paste.lol/mail-regexes-alerts/markup).

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-inbox-notifications/markup/embed"></iframe>

## Mapping categories as folders

I've tailored these rules to align with folders on a per topic basis. I have a broad `Financial` folder for things like receipts, bank statements and bills. That folder contains a few granular subfolders like `Deliveries`, `Media`, `Medical`, `Promotions` and so forth. All multi-step rules are set to filter messages when `any` of the tabled criteria matches.

The top level `Financial` rule [looks like this](https://cory.paste.lol/mail-regexes-financial/markup).

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-financial/markup/embed"></iframe>

`Deliveries` follow a similar pattern with rule sets intended to capture messages with package tracking information or other details. I kickstarted this rule by, naturally, referencing [this answer from StackOverflow](https://stackoverflow.com/a/5024011).

All of the regular expressions contained in this answer are matched against the `Body` of inbound messages before being forwarded to [Parcel Email](https://parcelapp.net/help/parcel-email.html)[^3]. These rules are supplemented by a few edge case rules targeted at the `Subject` field:

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-deliveries-edge-cases/markup/embed"></iframe>

Finally, I have a rule intended to catch anything that falls through the cracks[^4]:

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-deliveries-fail-safes/markup/embed"></iframe>

My `medical` and `media` rules follow a basic pattern that could be approximated using a per-line sender TLD match[^5]:

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-media/markup/embed"></iframe>

I'd recommend paring this down to match whichever `media` and `medical` providers email you.

This pattern of filtering and filing continues for several additional categories.

**Travel (non-forwarding)**

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-travel-non-forwarding/markup/embed"></iframe>

**Travel (forwarding)**

These are designed to capture confirmations sent by Southwest and are sent off to [Flighty](https://www.flightyapp.com) before being sorted.

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-travel-forwarding/markup/embed"></iframe>

**Annoying customer support follow-ups**

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-customer-service/markup/embed"></iframe>

**[Promotional messages (that you haven't unsubscribed from)](https://cory.paste.lol/mail-regexes-promotions/markup)**

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-promotions/markup/embed"></iframe>

**Social networking messages**

These I've left as a simple list wherein `any` included top level domain is filed away as I don't belong to many social networks and they change fairly infrequently.

**DMARC notifications (depending on how you have your policy record configured)**

<iframe style="width: 100%; height: 20em;" frameborder="0" onload="this.height=this.contentWindow.document.body.scrollHeight" src="https://cory.paste.lol/mail-regexes-dmarc/markup/embed"></iframe>

That covers _most_ of what I use to manage my mail (outside of anything particularly personal). I fully expect the regular expressions I'm using could stand to be refined and I plan on continuing to do just that. But, with that said, things have worked better than I expected so far and false positives/miscategorizations have been infrequent.

If you have any questions or suggestions I'm all ears. Feel free to [email me](mailto:fun.song5595@coryd.dev) or ping me on [Mastodon]().

[^1]: Before, well, _all that_.
[^2]: Fastmail has some helpful tips on regular expression rules here https://www.fastmail.help/hc/en-us/articles/360060591193-Rules-using-regular-expressions
[^3]: Fun fact, this is, apparently, no longer being actively developed — presumably because email, as we all know, is an absolute pleasure to parse and deal with.
[^4]: This rule doesn't forward over to Parcel as it typically captures secondary notices that either don't contain or duplicate the original tracking info.
[^5]: I know, I called this inefficient earlier.
