# Moonglue Studios — Official Website

A fast, static, multi-page website for Moonglue Studios. It has no build step and no framework. It works on any static host with HTTPS: Cloudflare Pages, Netlify, GitHub Pages or Vercel.

```
/                     index.html            Home
/about/               about/index.html      About Us
/games/               games/index.html      Our Games (Arrowtide at /games/#arrowtide)
/privacy-policy/      privacy-policy/…      Privacy Policy  ← permanent URL for Play Console
/contact/             contact/index.html    Contact Us
404.html, robots.txt, sitemap.xml, app-ads.txt, CNAME, _headers, site.webmanifest
assets/css/styles.css   all styling (colours are in :root at the top)
assets/js/main.js       mobile menu + contact form
assets/images/          logo, favicon, social image, game icon and screenshots
```

---

## 1. Before you publish: replace the placeholders

Use your editor's **Find and Replace in Files** across the whole folder:

| Find | Replace with | Where |
|---|---|---|
| `[YOUR OFFICIAL EMAIL]` | Your studio email, e.g. `contact@moongluestudios.com` | All pages (footer, contact, privacy) |
| `[YOUR SUPPORT EMAIL]` | Your support email (can be the same address) | Contact page |
| `[YOUR PRIVACY EMAIL]` | Your privacy contact email (can be the same) | Privacy policy, contact page |
| `YOUR_FORM_ID` | Your Formspree form ID (see step 3) | contact/index.html |
| `https://moongluestudios.com` | Your real domain, **only if it differs** | Canonical tags, sitemap, robots.txt |

Also check these:

1. **Privacy policy (important).** Open `privacy-policy/index.html` and read the `<!-- EDIT -->` comments. In the Arrowtide table, keep only the services the game really uses (AdMob, Firebase Analytics, Crashlytics). If it shows no ads, replace section 3 with "Our games do not display advertising." The policy has to match your **Data safety** form in Play Console. Update the "Last updated" date whenever you change it.
2. **Game images.** Replace the placeholder files in `assets/images/games/arrowtide/`:
   - `icon.svg` → your 512×512 icon. You can save it as `icon.png` and change `icon.svg` to `icon.png` in `index.html` and `games/index.html`.
   - `screenshot-1..4.svg` → real screenshots (1080×1920 PNG or WebP recommended). Update the file extensions the same way.
3. **Google Play link.** Every Play button points to
   `https://play.google.com/store/apps/details?id=com.moongluestudios.arrowtide`.
   This link works once the game is live on Google Play. Until then it shows a "not found" page, so publish the game first or make sure the listing is live before you rely on this link.
4. **Email addresses on your domain.** Use an address on the same domain as the site, such as `@moongluestudios.com`. Cloudflare Email Routing (free), Zoho Mail or Google Workspace can provide one. Google Play reviewers trust a matching domain more, and your personal address stays private.

## 2. Preview locally

```bash
cd moonglue-studios-website
python3 -m http.server 8000
# open http://localhost:8000
```

## 3. Make the contact form send email (Formspree, free tier)

1. Sign up at https://formspree.io and create a form that sends to your studio email.
2. Copy the form ID: the part after `/f/` in the endpoint, e.g. `xyzabcd`.
3. Replace `YOUR_FORM_ID` in `contact/index.html`.

Until you do this, the form still works: it opens the visitor's email app with the message already filled in, addressed to `[YOUR OFFICIAL EMAIL]`. The form has spam-bot protection (a hidden "honeypot" field), checks required fields, and shows accessible success and error messages.

*On Netlify, you can use Netlify Forms instead of Formspree: add `data-netlify="true"` to the `<form>` and set `action` to `/contact/`.*

## 4. Deploy to Vercel (recommended for this setup)

1. Push the contents of this folder to the root of your GitHub repository. `index.html` must be at the repository root.
2. Sign in at https://vercel.com using GitHub.
3. Choose **Add New → Project**, import the repository, and deploy.
4. For this plain HTML/CSS/JavaScript site, use **Framework Preset: Other**, **Build Command: empty**, and **Output Directory: `.`**. Set the Root Directory to the folder containing `index.html` (normally `./`).
5. The site will be available at the generated `*.vercel.app` address. `vercel.json` supplies Vercel-compatible security and asset-cache headers.

**Other hosts:**
- **Cloudflare Pages:** upload this folder using Direct Upload or connect the GitHub repository.
- **Netlify:** deploy the folder or connect the GitHub repository.
- **GitHub Pages:** configure Pages to deploy from the `main` branch and repository root. The `.nojekyll` file is included.

## 5. Connect your custom domain

1. **Buy the domain.** Check that `moongluestudios.com` is available at a registrar such as Cloudflare Registrar, Namecheap or Porkbun. If it's taken, choose a close alternative and update `CNAME`, `robots.txt`, `sitemap.xml` and the canonical tags.
2. **Point it at your host.**
   - *Cloudflare Pages:* open your Pages project, go to **Custom domains → Set up a domain**, and enter `moongluestudios.com`, then repeat with `www.moongluestudios.com`. If the domain's DNS is on Cloudflare, the records are created for you.
   - *Netlify:* go to **Domain management → Add domain**. At your registrar, add an `A` record for `@` pointing to `75.2.60.5` and a `CNAME` for `www` pointing to `<your-site>.netlify.app`.
   - *GitHub Pages:* add four `A` records for `@` (`185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`) and a `CNAME` for `www` pointing to `<username>.github.io`. Then tick **Enforce HTTPS**.
3. **Wait for HTTPS.** The host issues the certificate automatically, usually within minutes, occasionally up to 24 hours. Then make sure `http://` redirects to `https://` and that the plain domain and `www` both point to the same site.

## 6. Verify ownership in Google Search Console

1. Go to https://search.google.com/search-console and choose **Add property → Domain**, then enter `moongluestudios.com`.
2. Copy the `TXT` record Google gives you and add it to your domain's DNS. **This is the recommended method**: it covers every subdomain and survives site redesigns.
3. Click **Verify**, then go to **Sitemaps** and submit `https://moongluestudios.com/sitemap.xml`.

*Alternative:* the HTML-tag method. Uncomment the `google-site-verification` meta tag in `index.html` and paste your code there.

## 7. Connect the site to Google Play Console

- **Store settings → Store listing contact details → Website:** `https://moongluestudios.com`
- **Store settings → Store listing contact details → Email:** the same studio email shown on the site
- **App content → Privacy policy:** `https://moongluestudios.com/privacy-policy/`
- **Data safety → Delete data URL** (if Play asks for one): `https://moongluestudios.com/privacy-policy/#data-deletion`
- **Developer name** must be exactly **Moonglue Studios**, the name the site uses throughout.
- **If you use AdMob:** paste the line from **AdMob → Apps → app-ads.txt** into `app-ads.txt`. The file must be reachable at `https://moongluestudios.com/app-ads.txt`, and the website in Play Console must match this domain exactly.

## 8. Final checklist

- [ ] All `[YOUR … EMAIL]` placeholders replaced (search the folder to confirm none are left)
- [ ] Privacy policy matches the game's real SDKs and the Play Data safety form
- [ ] Real icon and screenshots in place
- [ ] Contact form sends to your inbox (send yourself a test message)
- [ ] `https://` works on the domain and `www`, and `http` redirects to it
- [ ] Domain verified in Search Console and sitemap submitted
- [ ] Play Console website, email and privacy policy URL set
- [ ] Play Store link works (the game is published)

## Adding another game later

1. Copy the `<section id="arrowtide">` block in `games/index.html` and change its id, text, images, package name and Play URL.
2. Add a matching sub-section to section 4 of `privacy-policy/index.html`.
3. Add the game to the "Game" dropdown in `contact/index.html`.
