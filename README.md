# Big Sales Funnel website

One-page static website for **Big Sales Funnel** (https://bigsalesfunnel.com): websites, social media posting and Google Business Profile cleanup for US restaurants and local small businesses.

Plain HTML + CSS + a few lines of inline JavaScript. No build step, no database, no PHP. Fonts (Fraunces and Inter) load from Google Fonts.

## File structure

```
.
├── index.html              Main one-page site
├── 404.html                Custom "page not found" page (uses root-absolute paths)
├── assets/
│   ├── css/style.css       All styles
│   └── img/logo-mark.svg   Logo mark
├── favicon.svg             Browser tab icon
├── favicon.ico             Fallback icon
├── favicon-32.png          32x32 PNG icon
├── apple-touch-icon.png    180x180 iPhone/iPad home-screen icon
├── og-image.png            1200x630 social sharing image (Facebook, WhatsApp, LinkedIn, X)
├── robots.txt              Search engine rules + sitemap link
├── sitemap.xml             Sitemap for https://bigsalesfunnel.com/
├── .htaccess               Apache/cPanel: HTTPS, www to non-www, 404, gzip, caching, security headers
├── .cpanel.yml             cPanel Git deployment instructions (edit USERNAME first)
├── .gitignore              Keeps previews, backups and OS files out of the repo
└── README.md               This file
```

All links in `index.html` are relative, so you can open it straight from the folder to preview it.

## A. Push to GitHub

1. Install Git (https://git-scm.com) and open a terminal in this folder.
2. Create the local repository:
   ```bash
   git init -b main
   git add .
   git commit -m "Initial Big Sales Funnel website"
   ```
3. On GitHub, click **New repository**. Name it e.g. `bigsalesfunnel-website`. Leave "Add a README / .gitignore / licence" **unticked** (they already exist). Choose Private or Public, then **Create repository**.
4. Connect and push (copy the URL GitHub shows you):
   ```bash
   git remote add origin https://github.com/YOUR-GITHUB-USERNAME/bigsalesfunnel-website.git
   git push -u origin main
   ```
   GitHub asks you to sign in. Use a Personal Access Token as the password (GitHub > Settings > Developer settings > Personal access tokens), or SSH if you have set up a key. With the GitHub CLI you can instead run `gh repo create bigsalesfunnel-website --private --source=. --push`.
5. Future changes: edit files, then `git add . && git commit -m "Describe the change" && git push`.

## B. Publish on cPanel (bigsalesfunnel.com)

Do these first, whichever method you choose:
- Make sure the domain's nameservers/DNS point to your hosting, and that `bigsalesfunnel.com` is the main domain (or an addon domain) in cPanel.
- Know your cPanel **username** (top-right of cPanel, or `/home/USERNAME` in File Manager).

### Method 1: cPanel Git Version Control (auto-copy on deploy)

1. Open `.cpanel.yml` and replace `USERNAME` with your cPanel username. If bigsalesfunnel.com is an addon domain with its own folder, change the path to that folder. Commit and push the change:
   ```bash
   git add .cpanel.yml && git commit -m "Set cPanel username" && git push
   ```
2. In cPanel, open **Git™ Version Control** > **Create**.
3. Switch **Clone a Repository** on. Enter:
   - **Clone URL:** your repo URL. For a private repo, use the SSH URL and add the cPanel server's public key as a deploy key in GitHub, or make the repo public.
   - **Repository Path:** `/home/USERNAME/repositories/bigsalesfunnel-website` (it must **not** be inside public_html).
   - **Repository Name:** `bigsalesfunnel-website`.
4. Click **Create**, then **Manage** next to the repo.
5. Open the **Pull or Deploy** tab. Click **Update from Remote**, then **Deploy HEAD Commit**. cPanel reads `.cpanel.yml` and copies the files into `public_html`.
6. Future updates: push to GitHub, then repeat step 5.

Troubleshooting: the deploy button only works if the repo has a clean working tree and `.cpanel.yml` is committed in the branch you deployed. If it says "cannot deploy", check for a typo in the username and that the file is valid YAML (spaces, not tabs).

### Method 2: File Manager upload of the zip

1. Download `bigsalesfunnel-deploy.zip` (the zip has the files at its root, including `.htaccess`).
2. In cPanel open **File Manager** > go into `public_html`.
3. Click **Settings** (top right) and tick **Show Hidden Files (dotfiles)**, then **Save**.
4. Delete the default `index.html` / `cgi-bin` placeholder files if cPanel created any (leave `.well-known` alone, AutoSSL uses it).
5. Click **Upload**, choose the zip, wait for 100%, then go back.
6. Right-click the zip in `public_html` > **Extract**. Extract into `/public_html`.
7. Check that `index.html`, `404.html`, `assets/` and `.htaccess` are directly inside `public_html` (not inside a sub-folder). Then delete the zip. Also delete `.cpanel.yml` if present (not needed for this method).
8. Visit https://bigsalesfunnel.com.

## C. Turn on SSL (AutoSSL) first

The `.htaccess` forces HTTPS, so the certificate must exist before you upload it.
1. cPanel > **SSL/TLS Status**.
2. Tick `bigsalesfunnel.com` and `www.bigsalesfunnel.com` and click **Run AutoSSL**. It usually takes a few minutes. DNS must already point to the host.
3. When both show a green padlock, the HTTPS and www redirects in `.htaccess` will work. If you see a redirect loop or certificate warning, temporarily rename `.htaccess` to `.htaccess.off`, fix SSL, then rename it back.

## D. After going live

- Test https://bigsalesfunnel.com, http:// and www. (all should end at https://bigsalesfunnel.com/), and a made-up URL such as /nothing-here (should show the 404 page).
- Check the sharing preview with a post in Facebook's Sharing Debugger (https://developers.facebook.com/tools/debug/) to refresh the og-image.
- Submit https://bigsalesfunnel.com/sitemap.xml in Google Search Console.
- If you change CSS and don't see it, hard refresh (Ctrl/Cmd + Shift + R). Static assets are cached for up to 30 days; rename or add `?v=2` to the CSS link in `index.html` when you make big style changes.

## TODO before / after launch

- [x] **Replace `funnelbigsales@gmail.com`** once the real email exists. Search `index.html` for `funnelbigsales@gmail.com` (it appears in the contact section and footer, with a TODO comment). Create the mailbox in cPanel > **Email Accounts**.
- [ ] **plaqueforlife.com screenshot:** the case-study image in `index.html` is a placeholder (`[PLACEHOLDER: plaqueforlife.com screenshot, add after client approval]`). Add a screenshot only after the client approves, e.g. save it as `assets/img/plaqueforlife.jpg` and replace the placeholder `<div class="shot" ...>` with an `<img>`.
- [ ] Add the real postal address to email footers if you start cold-emailing US businesses (CAN-SPAM). A PO box or virtual mailbox is fine.
- [ ] Optional: add Google Analytics or Search Console verification tags.
