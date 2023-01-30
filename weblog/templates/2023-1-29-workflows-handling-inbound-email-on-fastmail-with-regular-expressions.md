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

[I also have a rule containing regular expressions that also skips evaluations for login pin codes, meeting/appointment reminders and common security notices:](https://cory.paste.lol/mail-regexes-alerts/markup)

| Field   | Match                      | Criteria                                                                                                                                                |
| ------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Subject | matches regular expression | `([Pp][Ii][Nn]|[Vv]erif(y|ication|[Cc]onfirmation|[Oo]ne-[Tt]ime|[Oo]ne[Tt]ime)|one(-?| )time|single(-?| )use).*\b([Pp]asscode|[Nn]umber|[Cc]ode.*$).*` |
| Subject | matches regular expression | `(^.*[Uu]pcoming ([Aa]ppointment?|[Vv]isit).*$)`                                                                                                        |
| Subject | matches regular expression | `(^.*[Nn]ew.*([Ss]ign(in?|-in?|ed)|([Ll]og(in?|-in?|ged))))`                                                                                            |
| Subject | matches regular expression | `((^.*[Mm]eeting|[Vv]isit|[Aa]ppointment|[Ee]vent).*\b([Rr]eminder|[Nn]otification))`                                                                   |
| Subject | matches regular expression | `(^.*[Vv]erify.*([Dd]evice))`                                                                                                                           |
| Subject | matches regular expression | `(^.*[Aa]pple.*([Ii][Dd] was used to sign in))`                                                                                                         |
| Subject | matches regular expression | `(^.*([Cc]omputer|[Pp]hone|[Dd]evice).*([Aa]dded))`                                                                                                     |

## Mapping categories as folders

I've tailored these rules to align with folders on a per topic basis. I have a broad `Financial` folder for things like receipts, bank statements and bills. That folder contains a few granular subfolders like `Deliveries`, `Media`, `Medical`, `Promotions` and so forth. All multi-step rules are set to filter messages when `any` of the tabled criteria matches.

The top level `Financial` rule [looks like this:](https://cory.paste.lol/mail-regexes-financial/markup)

| Field   | Match                       | Criteria                                                                                                                                                                                                                                                |
| ------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Subject | matches regular expression | `(([Ii]nvoice|[Rr]ecei(pts?|ved)|[Pp]ayment|[Oo]rder|[Ss]ale|[Ss]ubscription|[Aa]uthori[zs]ed|[Ss]ent|[Ss]tatement|[Cc]onfirm(ation|ed)|[Bb]ooking|[Cc]omplete|[Cc]laim|[Tt]ransaction|[Bb]ill(ing)?|[Rr]enewal|[Ww]ithdrawal|[Pp]urchased?)(\b.*?|$))` |
| Subject | matches regular expression | `([Aa]mazon|receipt from).*\b(order|[Aa]pple).*`                                                                                                                                                                                                        |
| Subject | matches regular expression | `([Cc]redit).*\b([Aa]lert|[Ss]core|[Rr]eport).*`                                                                                                                                                                                                                                                        |

`Deliveries` follow a similar pattern with rule sets intended to capture messages with package tracking information or other details. I kickstarted this rule by, naturally, referencing [this answer from StackOverflow](https://stackoverflow.com/a/5024011).

All of the regular expressions contained in this answer are matched against the `Body` of inbound messages before being forwarded to [Parcel Email](https://parcelapp.net/help/parcel-email.html)[^3]. These rules are supplemented by a few edge case rules targeted at the `Subject` field:

| Field   | Match                       | Criteria                               |
| ------- | --------------------------- | -------------------------------------- |
| Subject | matches regular expression | `(^.*[Aa] shipment (from?|to)).*`      |
| Subject | matches regular expression  | `(^.*([Ww]as|[Hh]as) ([Ss]hipped).*$)` |

Finally, I have a rule intended to catch anything that falls through the cracks[^4]:

| Field              | Match                      | Criteria                                                            |
| ------------------ | -------------------------- | ------------------------------------------------------------------- |
| From email address | matches regular expression | `(^.*usps.*$|fedex.*$|narvar.*$|shipment-tracking.*$|getconvey.*$)` |
| From name          | matches glob pattern       | `?(?ed?x ?elivery ?anager|*?ed?x.com|?racking?pdates*)`             |
| Subject            | matches regular expression | `(^.*[Pp]ackage ([Hh]as [Bb]een?|[Ww]as) [Dd]elivered).*`           |

My `medical` and `media` rules follow a basic pattern that could be approximated using a per-line sender TLD match[^5]:

| Field              | Match                      | Criteria                                                                                                                                                                |
| ------------------ | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| From email address | matches regular expression | `([Dd]isneyplus.*$|[Nn]etflix.*$|^.*hulu.*$|[Hh][Bb][Oo][Mm]ax.*$|[Mm]ovies[Aa]nywhere.*$|i[Tt]unes.*$|7digital.*$|[Bb]andcamp.*$|[Rr]oku.*$|[Pp]lex.*$|[Pp]eacock.*$)` |

I'd recommend paring this down to match whichever `media` and `medical` providers email you.

This pattern of filtering and filing continues for several additional categories.

**Travel (non-forwarding)**

| Field   | Match                      | Criteria                                               |
| ------- | -------------------------- | ------------------------------------------------------ |
| Subject | matches regular expression | `(breakfast|lunch|dinner|hotel).*\b(reservation.*$).*` |
| Subject | matches regular expression | `([Yy]our).*\b(ride with|visit).*`                     |

**Travel (forwarding)**

These are designed to capture confirmations sent by Southwest and are sent off to [Flighty](https://www.flightyapp.com) before being sorted.

| Field   | Match                      | Criteria                                               |
| ------- | -------------------------- | ------------------------------------------------------ |
| Subject | matches regular expression | `([Ff]light).*\b(reservation).*` |
| Subject | matches regular expression | `([Yy]ou're going to).*\b(on).*`                     |

**Annoying customer support follow-ups**

| Field     | Match                | Criteria                              |
| --------- | -------------------- | ------------------------------------- |
| From name | matches glob pattern | `?(?ustomer)??(?are|?uccess|?upport)` |

**[Promotional messages (that you haven't unsubscribed from)](https://cory.paste.lol/mail-regexes-promotions/markup)**

| Field   | Match                      | Criteria                                                                       |
| ------- | -------------------------- | ------------------------------------------------------------------------------ |
| Subject | matches regular expression | `(.*[0-9]%).*(\[|-|off).*`                                                     |
| Subject | matches regular expression | `([Yy]ou're [Ee]ligible [Ff]or.*$)`                                            |
| Subject | matches regular expression | `([Ff]ree|[Ee]xpedited).*\b([Ss]hipping).*`                                    |
| Subject | matches regular expression | `([Hh]oliday|[Cc]hristmas).*\b(gift).*`                                        |
| Subject | matches regular expression | `([Bb]lack|[Cc]yber).*\b([Ff]riday|[Mm]onday).*`                               |
| Subject | matches regular expression | `([Ss]end|[Bb]uy|[Ss]are).*\b((g|e[gG])ift|card|present).*`                    |
| Subject | matches regular expression | `([Cc]learance|[Ff]inal|[Ll]ast [Cc]hance|[Ss]pecial).*\b([Ss]ale|[Oo]ffer).*` |
| Subject | matches regular expression | `([Ll]ast|[Ff]inal).*\b([Cc]all.*$|[Cc]hance.*$|[Ss]ale.*$|[Dd]iscount.*$).*`  |
| Subject | matches regular expression | `([Cc]ash.*([Rr]eward(?|s)))`                                                  |
| Body    | matches regular expression | `(.*[0-9]%).*(\[|-|off).*`                                                     |

**Social networking messages**

These I've left as a simple list wherein `any` included top level domain is filed away as I don't belong to many social networks and they change fairly infrequently.

**DMARC notifications (depending on how you have your policy record configured)**

| Field              | Match                      | Criteria                   |
| ------------------ | -------------------------- | -------------------------- |
| Subject            | matches regular expression | `((^.*dmarc.*$)(\b.*?|$))` |
| From email address | matches regular expression | `((^.*dmarc.*$)(\b.*?|$))` |

That covers _most_ of what I use to manage my mail (outside of anything particularly personal). I fully expect the regular expressions I'm using could stand to be refined and I plan on continuing to do just that. But, with that said, things have worked better than I expected so far and false positives/miscategorizations have been infrequent.

If you have any questions or suggestions I'm all ears. Feel free to [email me](mailto:fun.song5595@coryd.dev) or ping me on [Mastodon](https://social.lol/@cory).

[^1]: Before, well, _all that_.
[^2]: Fastmail has some helpful tips on regular expression rules here https://www.fastmail.help/hc/en-us/articles/360060591193-Rules-using-regular-expressions
[^3]: Fun fact, this is, apparently, no longer being actively developed — presumably because email, as we all know, is an absolute pleasure to parse and deal with.
[^4]: This rule doesn't forward over to Parcel as it typically captures secondary notices that either don't contain or duplicate the original tracking info.
[^5]: I know, I called this inefficient earlier.
