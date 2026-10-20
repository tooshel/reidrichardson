# reidrichardson.com

The website of painter Reid Richardson: home, Selected Works, Videos, Bio,
Galleries. In 2026 it moved off WordPress and became a plain static site
hosted free on GitHub Pages.

## Why it moved

**WordPress was more machine than this site needs.** It's a database, PHP,
a theme, a page builder and a stack of plugins, all of which need updates
forever, all running on a paid server, to show what amounts to a handful of
pages that rarely change. The page that changes most, Selected Works, isn't
even stored in WordPress. It's an embed from Artwork Archive, which keeps
running no matter where the site lives.

**The bigger reason is that changing a website works differently now.** The
old way: remember the WordPress password, log in, find the right page in the
page builder, poke at it until it looks right, and hope nothing else moved.
The new way: say what you want in plain English ("add my new show to the
bio," "swap the homepage painting") and an AI assistant makes the edit,
previews it, and publishes it. **AI can make the change faster than you can
remember your WordPress password.** Every change is also saved in history,
so any edit can be undone exactly.

What else you get:

- **Faster pages.** There's nothing to assemble per visit; GitHub serves
  the finished files straight from its global network.
- **Nothing to hack.** No login page, no database, no plugins to patch.
- **No hosting bill.** GitHub Pages is free for public sites.
- **No updates, ever.** Static files don't go out of date.

## What changed in the move

The site looks and reads the same, Selected Works included, with a few
upgrades made along the way:

- **Videos page added**, featuring *South Mountain Thunderhead*, filmed by
  The Signature Gallery.
- **Quieter navigation.** The stock gray WordPress-theme bar is gone; the
  nav and footer now use the same small, gallery-style lettering.
- **Better sharing.** Sending someone the link now shows a preview card with
  one of Reid's paintings instead of a bare URL.
- **Cleaner bio.** Links to venues that no longer exist (MySpace, Art One,
  Signature Laguna) are unlinked, with the history text kept; broken gallery
  links now point at the current pages.
- **Rescued homepage art.** The homepage slideshow had been loading its
  paintings from the old server, which is shutting down. Those images now
  live inside this site.
- **A proper "page not found" page** with the signature logo.
- Same web addresses as before (`/selected-works/`, `/bio/`, and so on), so
  old links and bookmarks keep working.

## Making changes

Ask Sheldon. A text like "add the October show at Globe Fine Art to the bio"
is all it takes; he makes the change with an AI assistant and it goes live
within minutes.

For the technically curious: every page is a plain HTML file in this repo
(`bio/index.html`, `galleries/index.html`, and so on). Commit a change to the
`main` branch and GitHub Pages publishes it automatically, usually within a
minute.

## Switching the domain over (DNS)

The new site is ready. Until the domain is switched, it's at
**https://tooshel.github.io/reidrichardson/**, and
**www.reidrichardson.com** still serves the old WordPress site.

> **This switch must happen before the old server is shut down.** Today
> `www.reidrichardson.com` points at the old host (by way of
> `www.wpsplit.com`). When that host is cancelled, the old site stops
> working, so the change below has to come first.

Reid's DNS is managed at **GoDaddy** (the nameservers are
`ns51.domaincontrol.com` and `ns52.domaincontrol.com`). Whoever has the
GoDaddy login makes **one change**.

### Step 1: Sheldon connects the domain on GitHub (first)

Sheldon adds `www.reidrichardson.com` as the custom domain in this repo's
GitHub Pages settings. Nothing visible changes for visitors yet. The only
effect is that the github.io preview link starts redirecting to
`www.reidrichardson.com`, which is still the old site.

### Step 2: Change one record at GoDaddy

In GoDaddy, open **reidrichardson.com → DNS** and edit the existing `www`
record:

| Type  | Name  | Old value          | New value               |
|-------|-------|--------------------|-------------------------|
| CNAME | `www` | `www.wpsplit.com`  | **`tooshel.github.io`** |

That's the whole change. Keep the TTL at its default.

### Do NOT touch these

- **The MX records** (`aspmx.l.google.com`, `alt1.aspmx.l.google.com`,
  `alt2.aspmx.l.google.com`, `aspmx2.googlemail.com`,
  `aspmx3.googlemail.com`). They deliver Reid's email through Google.
  Changing or deleting them breaks email.
- **The bare-domain forwarding.** `reidrichardson.com` (without `www`)
  already forwards to `https://www.reidrichardson.com/`. It keeps working
  as-is and will land visitors on the new site automatically.

### Step 3: Wait, then Sheldon locks in HTTPS

- The change usually takes effect within 5 minutes to an hour.
- Once GitHub sees the new record, it issues a security certificate for the
  domain automatically. **For a short window (usually under an hour),
  `https://www.reidrichardson.com` may show a browser security warning**
  while the certificate is created. This is normal, so do the switch at a
  quiet time.
- When the certificate is ready, Sheldon turns on "Enforce HTTPS" and
  everything is permanent.

### Checklist after the switch

- [ ] `https://www.reidrichardson.com` loads with no warning and shows the
      new site (the nav includes **Videos**)
- [ ] `reidrichardson.com` (no www) lands on the same site
- [ ] Selected Works shows the Artwork Archive gallery
- [ ] Email to Reid still arrives

### If something goes wrong

While the old server still exists, setting the `www` record back to
`www.wpsplit.com` restores the old site exactly. After the old server is
gone, there's nothing to go back to, which is one more reason to switch
before the shutdown.

### Optional, later

If the GoDaddy forwarding is ever removed, replace it with these four `A`
records on the bare domain (`@`): `185.199.108.153`, `185.199.109.153`,
`185.199.110.153` and `185.199.111.153`. GitHub then sends bare-domain
visitors to `www` by itself. This isn't needed today.
