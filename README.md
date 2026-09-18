# safari-feed-blocker

One CSS file that hides the feeds on Reddit, Facebook and Medium, and every
social-media result on Google, in Safari on the Mac, using nothing but Safari's own style-sheet setting. No extension, no
account, nothing installed, nothing that phones home. Everything you go to a
site *for* still works: the post you open, its comments, your inbox, your
groups, your own profile, search. What disappears is the endless column that
keeps you there after you have done what you came to do.

It also turns Medium dark when your Mac is dark, since Medium has no dark
theme of its own.

## Install

1. Download `user.css` from this repo (or clone the repo) and keep the file
   somewhere it will stay, such as `~/Documents/user.css`. Safari reads it from
   wherever you leave it, so moving the file later breaks the link.
2. In Safari, open **Settings** (⌘,), then the **Advanced** tab.
3. Beside **Style sheet**, choose **Other…** and pick the file.

That is the whole installation. Reload any open Reddit, Facebook, Medium or
Google tab and the feed is gone.

If you would rather do it from Terminal:

```sh
defaults write com.apple.Safari UserStyleSheetEnabled -bool true
defaults write com.apple.Safari UserStyleSheetLocationURLString "file://$HOME/Documents/user.css"
```

then quit and reopen Safari.

## Switch it off, or drop a site

- **Everything off:** set **Style sheet** back to **None** in the same pane.
  Nothing else changes; the file just stops being read.
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
The Recent Posts column beside the home feed. What stays: the community's
highlights row and pinned posts, any post you open and its comments, your
profile, your inbox and search.

**facebook.com.** The news feed and the Stories tray on the home page;
Reels, whether opened from the Reels tab or from a shared link, since the
page's main column is emptied and only the rail is left; and on the Friends
page the Home view's friend requests and "People you may know" suggestions,
so that the page opens empty and All friends is one click away in the rail. What stays: the composer, the menu on the left, the column on the
right, and every other page. Groups and profiles keep their posts, because the
feed rule applies only to the page that carries the Stories tray.

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

**Dark mode on Medium** follows the Mac's appearance (System Settings >
Appearance, or the Display tile in Control Center; Auto turns it on at
sunset). The page is inverted and pictures are inverted back. If you do not
want it, delete the block that begins `@media (prefers-color-scheme: dark)`.

## Limits

- Safari on the Mac only. Safari on iPhone and iPad has no user style sheet,
  and the phone apps are out of reach altogether.
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
elements (`shreddit-feed`, `recent-posts`), which are easy. Facebook and
Medium randomise their class names, so the rules there key on things that
survive a rebuild: `role`, `aria-*` and `data-*` attributes, and the shape of
the markup around a stable heading. One rule of CSS to remember while editing:
`:has()` cannot be nested inside another `:has()`, and a selector that does
so fails silently.

## License

MIT.
