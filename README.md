# safari-feed-blocker

One CSS file that hides the feeds on Reddit, Facebook and Medium, the pictures
on Instagram, and every social-media result on Google, in Safari on the Mac and
in Safari on iPhone. On the Mac it needs nothing but Safari's own style-sheet
setting: no extension, no account, nothing installed, nothing that phones home.
On the phone the same file, unchanged, runs through Userscripts, a free and
open-source Safari extension. Everything you go to a site *for* still works:
the post you open, its comments, your inbox, your groups, your own profile,
search. What disappears is the endless column that keeps you there after you
have done what you came to do.

Instagram is the one deliberate exception. The section for it assumes you have
no account and open Instagram only from a link somebody sent you, so it keeps
what a profile says in words and takes away every picture, and a post or a reel
opens empty.

It also turns Medium dark when your Mac or your phone is dark, since Medium has
no dark theme of its own.

## Install on the Mac

1. Download `user.css` from this repo (or clone the repo) and keep the file
   somewhere it will stay, such as `~/Documents/user.css`. Safari reads it from
   wherever you leave it, so moving the file later breaks the link.
2. In Safari, open **Settings** (⌘,), then the **Advanced** tab.
3. Beside **Style sheet**, choose **Other…** and pick the file.

That is the whole installation. Reload any open Reddit, Facebook, Instagram,
Medium or Google tab and the feed is gone.

If you would rather do it from Terminal:

```sh
defaults write com.apple.Safari UserStyleSheetEnabled -bool true
defaults write com.apple.Safari UserStyleSheetLocationURLString "file://$HOME/Documents/user.css"
```

then quit and reopen Safari.

## Install on iPhone

Safari on iPhone has no style-sheet setting, so the same file runs through
[**Userscripts**](https://apps.apple.com/us/app/userscripts/id1463298887), a
free, open-source extension that appends a style sheet to the sites you name.
The `==UserStyle==` header at the top of `user.css` is what it reads; Safari on
the Mac passes over the header, because it is an ordinary CSS comment. One file
serves both machines.

1. Install **Userscripts** from the App Store.
2. Open **Settings > Apps > Safari > Extensions**, tap **Userscripts**, and turn
   on **Allow Extension**. Then set its permission for **All Websites** to
   **Allow**, so it runs on the five sites without asking each time.
3. Open the Userscripts app once and leave its save location at the default,
   which is the app's own folder. The Files app shows that folder under **On My
   iPhone > Userscripts**; the app writes a demo user script into it on first
   launch, which is what makes it visible there, and you can delete that demo
   once `user.css` is in.
4. Put `user.css` in that folder, once, by whichever route reaches the phone
   most easily. AirDrop it from your Mac. Or open
   [the raw file](https://raw.githubusercontent.com/megan-holstein/safari-feed-blocker/main/user.css)
   in Safari and choose **Save to Files** from the share sheet. Or, if you keep
   a copy in iCloud Drive, copy it across in the Files app — copy it into the
   Userscripts folder, rather than pointing Userscripts at the iCloud folder,
   for the reason two paragraphs down.
5. Open the Userscripts app again, or the extension's popup in Safari, and wait
   for **Feed Blocker** to appear in its list. This step is not optional: the
   extension keeps its own table of which file runs on which site, and it
   rebuilds that table only when the app or the popup loads the list, never
   when a file lands in the folder. A file the list has not shown yet injects
   nothing. Then reload the page.

To confirm it is running, open one of the five sites, tap the button on the left
of the address field, and choose **Userscripts**: the extension lists what it is
running on the page, and **Feed Blocker** is in the list.

**Later versions come down from GitHub, so that first copy is the only one you
move by hand.** `user.css` declares a `@version` and an `@updateURL` naming the
raw file in this repo, which is all Userscripts requires to treat a file as
updatable. Open the extension's popup, tap the cloud button along the top, and
choose **Check Updates**: the extension fetches the update URL, reads the
`@version` in the header it finds there, and lists **Feed Blocker** when that
number is higher than the one on the phone — it compares the numbers one at a
time, so 1.0.10 counts as newer than 1.0.9. **Update** beside the entry, or
**Update All**, then rewrites the phone's copy from `@downloadURL`, which names
the same raw file. Nothing to pull, nothing to copy again. Reload the page and
the new rules are running, since the extension reads the file itself at every
injection; only a change to the `@match` lines asks for the app or the popup to
load the list again. An edit that does not raise `@version` never travels at
all, which is why every commit here that touches `user.css` raises it.

That check is a tap rather than a schedule, for the moment. Userscripts used to
run it on its own whenever the popup opened; version 4.8.6 ships that code
commented out over
[issue #894](https://github.com/quoid/userscripts/issues/894), and on the
development branch, where it has returned, it stays off until you choose an
interval — 1, 3, 7, 15 or 30 days — in the extension's settings. The app's own
editor offers a sync button per file as well, but it compares the two versions
as text rather than number by number, so the popup is the one to trust.

**Do not point Userscripts at a folder in iCloud Drive.** It is the obvious
idea — drop the file in from the Mac and every later edit reaches the phone on
its own — and today it fails. The extension reads its folder synchronously,
while Apple serves iCloud Drive through FileProvider, which answers
asynchronously, so the extension finds nothing to list: no **Feed Blocker**, an
empty list, and no injection, even with every file downloaded and the folder
set to **Keep Downloaded**. The maintainer describes the same cause and the
same symptoms in [#424](https://github.com/quoid/userscripts/issues/424),
[#728](https://github.com/quoid/userscripts/issues/728) and
[#814](https://github.com/quoid/userscripts/issues/814), and the repair waits
on a backend refactor that has not shipped. Until it does, leave the save
location alone and let the update check above carry the changes.

One thing the sheet cannot reach: a link to Reddit or Facebook opens that site's
own app when you have the app installed, and the sheet governs Safari only. Open
such a link with **Open in Safari**, or keep the app off the phone.

## Switch it off, or drop a site

- **Everything off:** set **Style sheet** back to **None** in the same pane.
  Nothing else changes; the file just stops being read. On the phone, turn
  **Allow Extension** off under Settings > Apps > Safari > Extensions.
- **One site only:** open `user.css` in any text editor and delete that site's
  section. Each section begins with a comment line naming the site. Safari
  picks the edit up on the next reload.
- **Take a break from the blocker for one visit:** there is no toggle, on
  purpose. Turning it off means opening Settings, which is enough friction to
  make you notice you are doing it.

## What each section hides

**reddit.com.** The feed on the home page, r/popular and r/all. On a
community page, every post the moderators have not pinned, together with the
ads and the loaders between them, so the page never fetches a second page.
The Recent Posts column beside the home feed. On a search, the post results
and the "People also search for" suggestions, leaving the tabs and the
Communities and Profiles column, which is what a search is for once the
posts are gone. What stays elsewhere: the community's
highlights row and pinned posts, any post you open and its comments, your
profile, your inbox and search.

**facebook.com.** The news feed and the Stories tray on the home page;
Reels, whether opened from the Reels tab or from a shared link, since the
page's main column is emptied and only the rail is left; and on the Friends
page the Home view's friend requests and "People you may know" suggestions,
so that the page opens empty and All friends is one click away in the rail. What stays: the composer, the menu on the left, the column on the
right, and every other page. Groups and profiles keep their posts, because the
feed rule applies only to the page that carries the Stories tray.

**instagram.com.** On a profile: the profile picture, the story highlights, the
whole grid under the Posts, Reels and Tagged tabs, and the "Accounts you might
like" carousel beneath it. What stays is everything the page says in words —
the username, the follower and following counts, the display name, the category
line, the bio and its link — and the tabs themselves. A post or a reel opens
empty, whichever address the link carries (`/p/`, `/reel/`, `/reels/`, or the
`/<username>/p/` and `/<username>/reel/` forms): the main column goes whole, so
no picture, no video, no carousel, no player, and the caption goes with them.
Explore and a topic page are one grid under two addresses and are emptied the
same way. Two pages never reach the sheet at all, because Instagram never draws
them for a signed-out visitor: a profile's Tagged tab redirects to the login
page, and instagram.com itself is the login form. Signed in, the home feed is
a page this section does not cover.

**google.com.** Any web result that links to a social-media site, along
with its site-links and the "More results from" line under it, and the AI
Overview whenever it cites one, since an answer built on those sources is not
one worth reading. The sites: Reddit, Facebook, Instagram, Threads, X and
Twitter, TikTok, Snapchat, Pinterest, LinkedIn, Tumblr, Bluesky and Quora.
YouTube is left out, because most of what it answers on Google is a how-to.
The list sits at the top of the Google section as one line per site, so
adding or striking one is a one-line edit. Two things follow from hiding a
block whole: a "People also ask" or "Discussions and forums" box goes if any
answer in it comes from one of those sites, and a `site:reddit.com` search
shows an empty page, so run that one with the sheet off.

**medium.com.** The home feed under the For you and Featured tabs and the
column beside it (Staff Picks, Recommended topics, Who to follow, Reading
list), and the "More from" and "Recommended from Medium" blocks under a story.
What stays: the story itself and its responses, your profile and its stories,
your drafts and published lists, stats and notifications.

**Dark mode on Medium** follows the appearance of whatever you are reading on —
on the Mac, System Settings > Appearance, or the Display tile in Control Center,
where Auto turns it on at sunset; on the phone, Settings > Display & Brightness.
The page is inverted and pictures are inverted back. If you do not want it,
delete the block that begins `@media (prefers-color-scheme: dark)`.

**On the phone.** Reddit and Medium carry over as they stand. Reddit serves a
phone the same app it serves a Mac, on the same host, with the same custom
elements and the same route names, so every Reddit rule fires there unchanged.
Medium's feed cards carry the same `source=` tags on the phone and go to the
card rule, and the column beside the feed has no phone equivalent to hide, since
the phone lays the page out in one column. Instagram took two edits, both of
them in this file: a post carries no `[role="main"]` on a phone, so the rule
names the `<article>` the phone draws instead as well; and on a profile an
`<hr>` sits between the tabs and the grid, so the rule reaches the grid as a
later sibling as well as the next one. Facebook and Google are the two nobody
has checked on a phone — see Limits.

## Limits

- **The Mac needs no extension; the phone needs Userscripts.** Safari on iPhone
  and iPad has no style-sheet setting of its own, which is what the header at
  the top of `user.css` is for. The phone apps are out of reach on either
  machine.
- **Facebook and Google are unverified on the phone.** Signed out, a phone is
  served a login form on m.facebook.com and never reaches the news feed, so the
  Facebook rules could only be checked signed in, and they were not. Google
  answered every automated request from the network this was tested on with a
  CAPTCHA, so whether a phone's results still carry `#rso`, `[data-rpos]` and
  `#m-x-content` is an open question. Neither section was guessed at: both stand
  exactly as the Mac verified them. If one of them misses on your phone, that is
  why, and an issue saying what you see is welcome.
- **The Mac and the phone load the file at different origins.** Safari's own
  setting loads it at *user* origin, where `!important` outranks the page's own
  `!important`. Userscripts appends it as an ordinary author style sheet, where
  a page's rule can win. Every rule here already declares
  `display: none !important`, which is enough against these five sites as they
  stand — but a rule that works on the Mac and fails on the phone, with no
  change to the site's markup, is the signature of that difference.
- The sheet applies to every page you visit, so each rule is written to match
  things only its own site has. If some other site ever loses an element, the
  culprit is one of these rules, and deleting the section fixes it.
- Sites rename their markup from time to time, and when one does its feed
  comes back until the rule is updated. Pull the repo again, or open an issue
  saying which site and what you see.

## When a site changes

If you want to repair a rule yourself: open the page, choose **Develop > Show
Web Inspector** (turn the Develop menu on under Settings > Advanced first),
and read what the feed's container is called now. Reddit uses named custom
elements (`shreddit-feed`, `recent-posts`), which are easy. Facebook,
Instagram and Medium randomise their class names, so the rules there key on
things that survive a rebuild: `role`, `aria-*` and `data-*` attributes, the
alt text on a picture, the head's own `og:` and `al:` metadata, and the shape
of the markup around a stable heading. One rule of CSS to remember while editing:
`:has()` cannot be nested inside another `:has()`, and a selector that does
so fails silently.

## License

MIT.
