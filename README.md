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
`ns51.domaincontrol.com` and `ns52.domaincontrol.com`). The switch is done
on a call with Sheldon (GitHub side) and Reid (GoDaddy side). It takes one
record change, and most of the hour is waiting.

### Before the call

- **Sheldon:** signed in to GitHub as `tooshel`, with this page open:
  https://github.com/tooshel/reidrichardson/settings/pages
- **Reid:** GoDaddy username, password, and whatever device gets his
  two-factor codes. Being on a computer with screen sharing makes it easier.
- Pick a quiet time and allow about an hour.

### Step 1 (Sheldon): add the domain on GitHub

1. On the Pages settings page above, find the **Custom domain** box.
2. Type `www.reidrichardson.com`. Include `www`; leave out `https://` and
   any trailing slash. Click **Save**.
3. GitHub starts a DNS check, and **it will fail at first. That's expected**,
   because GoDaddy hasn't been changed yet. Leave the domain in place.
4. GitHub makes a small commit to this repo adding a file named `CNAME`
   (contents: `www.reidrichardson.com`). Leave it alone; that file is how
   GitHub remembers the domain.
5. The **Enforce HTTPS** checkbox will be grayed out. Also expected.

Nothing changes for visitors yet. The github.io preview link now redirects
to `www.reidrichardson.com`, which is still the old site until Step 2.

### Step 2 (Reid): change one record at GoDaddy

1. Sign in at godaddy.com and go to **My Products**. Next to
   **reidrichardson.com**, choose **DNS** (sometimes labeled **Manage DNS**).
2. In the list of records, find the row with **Type** `CNAME` and **Name**
   `www`. Click its edit (pencil) icon.
3. Change the value, then click **Save**:

   | Type  | Name  | Old value          | New value               |
   |-------|-------|--------------------|-------------------------|
   | CNAME | `www` | `www.wpsplit.com`  | **`tooshel.github.io`** |

   - **Edit the existing record. Don't add a second `www` record**; there
     can only be one.
   - Keep the TTL at its default. A trailing dot on the value
     (`tooshel.github.io.`) is fine either way.

**Do NOT touch anything else on that page**, especially:

- **The MX records** (`aspmx.l.google.com`, `alt1.aspmx.l.google.com`,
  `alt2.aspmx.l.google.com`, `aspmx2.googlemail.com`,
  `aspmx3.googlemail.com`). They deliver Reid's email through Google.
  Changing or deleting them breaks email.
- **The bare-domain forwarding.** `reidrichardson.com` (without `www`)
  already forwards to `https://www.reidrichardson.com/`. It keeps working
  as-is and will land visitors on the new site automatically.

### Step 3 (both): watch it switch over

1. **Check that the change is visible**, no tools needed. Open this link
   and look for the `"data"` value:
   https://dns.google/resolve?name=www.reidrichardson.com&type=CNAME
   - `www.wpsplit.com.` means the change hasn't spread yet. Refresh every
     few minutes.
   - `tooshel.github.io.` means the world sees the new record. This usually
     takes 5 minutes to an hour.
2. **Sheldon:** refresh the GitHub Pages settings page. The DNS check should
   now pass, and GitHub starts creating the security certificate on its own.
3. **During the certificate wait**, `https://www.reidrichardson.com` may show
   a browser security warning. That's normal and usually clears within an
   hour.
4. **Sheldon:** once the **Enforce HTTPS** checkbox becomes clickable, tick
   it. The switch is done.

### If it stalls

- **The dns.google link still says `www.wpsplit.com` after an hour.** The
  GoDaddy edit didn't save, or a second `www` record was added. Reid
  re-opens the GoDaddy DNS page and checks that exactly one `www` CNAME
  exists and it says `tooshel.github.io`.
- **dns.google shows `tooshel.github.io`, but GitHub's DNS check or the
  Enforce HTTPS box is still stuck after an hour.** Kick GitHub to look
  again: in Pages settings, click **Remove** next to the custom domain, then
  type `www.reidrichardson.com` back in and click **Save**. This has fixed the
  same stall on other sites in this migration. Each remove and re-add makes
  GitHub add another `CNAME` commit, so pull before editing the repo again.
- **Undo everything:** while the old server still exists, setting the
  GoDaddy `www` record back to `www.wpsplit.com` restores the old site
  exactly. After the old server is gone, there's nothing to go back to,
  which is one more reason to switch before the shutdown.

### Checklist after the switch

- [ ] `https://www.reidrichardson.com` loads with no warning and shows the
      new site (the nav includes **Videos**)
- [ ] `reidrichardson.com` (no www) lands on the same site
- [ ] Selected Works shows the Artwork Archive gallery
- [ ] Email to Reid still arrives (send him a test message)

### Optional, later

If the GoDaddy forwarding is ever removed, replace it with these four `A`
records on the bare domain (`@`): `185.199.108.153`, `185.199.109.153`,
`185.199.110.153` and `185.199.111.153`. GitHub then sends bare-domain
visitors to `www` by itself. This isn't needed today.
