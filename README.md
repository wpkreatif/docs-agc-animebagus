# AGC Anime Streaming Web Application

A lightweight, fast, and feature-rich Auto-Generated Content (AGC) Anime streaming CMS built with PHP, Tailwind CSS, and optimized for speed and SEO.

## 🌟 Features

- **Blazing Fast Performance**: Minimal PHP routing with caching systems built-in.
- **Admin Panel Control**: Fully dynamic configuration powered by an intuitive admin panel (PWA, SEO, Social Links, Ads).
- **SEO & Social Ready**: Built-in OpenGraph, Twitter Cards, Schema.org JSON-LD snippets, and automated XML Sitemap generation.
- **Monetization Optimized**: Direct Link URL integration with advanced cooldown mechanisms and floating ad placements.
- **PWA Ready**: Installable Progressive Web Application support out of the box.
- **Secure Architecture**: Proxied AJAX requests to hide backend API origins, PRG patterns, and strict security headers.

---

## 📋 Requirements

Before installing, make sure your server meets the following requirements:

1. **PHP:** Version 8.1 or higher.
2. **Web Server:** Nginx or Apache.
3. **PHP Extensions:**
   - `cURL` (for fetching remote API data)
   - `JSON` (for reading/writing settings)
   - `fileinfo` (for image uploads)
   - `mbstring` (for string manipulation)
4. **Permissions:**
   - Proper read/write permissions for the `settings.json` file.
   - Writable permissions (`755` or `777` depending on your setup) for the `/images` and `/cache` directories.

---

## 🚀 Installation Guide

### Step 1: Upload the Source Code

Upload all files and folders to your server's root directory (e.g., `/var/www/html/` or `public_html`).

### Step 2: Configure Permissions

Ensure that the application can write to the necessary directories and files.

```bash
chmod -R 755 images/
chmod -R 755 cache/
chmod 666 settings.json
```

### Step 3: Web Server Configuration

**If using Nginx (Recommended):**
Make sure to apply the provided `nginx.conf` routing rules to your server block to enable clean URLs.

```nginx
location / {
    try_files $uri $uri/ /index.php?$query_string;
}

# Protect internal files
location ~ ^/(settings\.json|functions\.php|config\.php) {
    deny all;
}
```

**If using Apache:**
Create a `.htaccess` file in the root directory with the following Rewrite rules to enable clean URLs:

```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?route=$1 [L,QSA]
```

### Step 4: Admin Panel Setup

1. Open `functions.php` and locate the Admin Setup section.
2. Check or modify your credentials:
   ```php
   define('ADMIN_USERNAME', 'admin');
   define('ADMIN_PASSWORD', '$2y$10$YourBcryptHashHere'); // Default is 'admin' hashed
   ```
3. Default Login:
   - **URL:** `yourdomain.com/admin.php`
   - **User:** `admin`
   - **Pass:** `admin`

### Step 5: Configure Application

Once logged in to the Admin Panel, navigate through the settings to upload your Logo, Favicon, configure API endpoints, and insert your advertisement scripts.

_(Note: Don't forget to configure your SEO metadata and Social links in the integrations tab!)_

---

## 🗂️ File Tree Structure

```text
.
├── 404.php                 # Dynamic 404 Error Page
├── admin.php               # Admin Control Panel UI & Logic (PRG protected)
├── ajax.php                # Secure proxy endpoint for frontend JS requests
├── bookmark.php            # User bookmarks page
├── config.php              # Global configuration loader from settings.json
├── css/
│   ├── styles.css
│   └── styles.min.css      # Minified Tailwind & Custom CSS
├── footer.php              # Global footer layout & Advertisement Logic
├── functions.php           # Core application functions, constants, and Security Headers
├── genre.php               # Genre taxonomy routing
├── header.php              # Global header layout, SEO & OpenGraph tags
├── history.php             # User watch history page
├── index.php               # Main front controller and router
├── js/
│   ├── app.js              # Global site interactions & plugins
│   ├── bookmark.js
│   ├── history.js
│   ├── home.js             # Homepage dynamic content fetching
│   ├── pwa.js
│   ├── sidebar.js          # Sidebar taxonomies loading
│   ├── single.js
│   └── watch.js            # Video player interactions
├── latest.php              # Latest episodes feed
├── manifest.php            # Dynamic PWA Manifest Generator
├── movie.php               # Movies feed
├── nginx.conf              # Required Nginx rewrite rules
├── ongoing.php             # Ongoing series feed
├── pages/                  # Static pages
│   ├── contact.php
│   ├── dmca.php
│   └── terms.php
├── proxy/                  # Advanced streaming proxy handlers
│   ├── index.php
│   ├── stream.php
│   └── watch.php
├── random.php              # Random anime generator
├── rekomendasi.php         # Recommendation widget
├── robots.txt              # Search engine crawler directives
├── search.php              # Search results routing
├── season.php              # Seasons taxonomy routing
├── settings.json           # Primary configuration database (Written by admin.php)
├── sidebar.php             # Global sidebar layout
├── single.php              # Anime details page
├── sitemap_generator.php   # Automated comprehensive XML Sitemap generator
├── studio.php              # Studios taxonomy routing
└── watch.php               # Anime streaming page & episode navigation
```

---

## 💰 Sample Advertisement Codes

Here are some sample HTML structures you can paste into the Admin Panel Ads fields. Make sure to replace `#` or image links with your actual Ad Network scripts.

**Banner Ad (530x90 or 728x90) Example:**

```html
<a
  class="banner-responsive"
  href="https://your-ad-link.com"
  target="_blank"
  rel="nofollow noopener"
>
  <img
    src="https://placehold.co/728x90?text=Ads+728x90&font=oswald"
    style="max-width: 728px; width: 100%"
    alt="banner"
  />
</a>
```

**Sidebar Ad (300x250 or 300x300) Example:**

```html
<a
  class="banner-responsive"
  href="https://your-ad-link.com"
  target="_blank"
  rel="nofollow noopener"
>
  <img
    src="https://placehold.co/300x250?text=Ads+300x250&font=oswald"
    style="max-width: 300px; width: 100%"
    alt="banner"
  />
</a>
```

**Floating Ad Example:**

```html
<!-- Usually provided by Popunder/Direct Link networks as a script tag -->
<script type="text/javascript" src="//pl1234567.pu.com/7g/3h/9d.js"></script>
```

---

## ⚙️ Automating `sitemap_generator.php` via Cronjob

To keep your SEO up to date, you should automate the execution of `sitemap_generator.php` so it fetches new anime episodes and movies constantly. Below is how to set up the Cronjob across popular hosting control panels.

### 1. aaPanel

1. Go to the **Cron** menu on the left sidebar.
2. Select **Type of Task:** `Shell Script`.
3. Set **Execution Cycle:** `N Minutes` -> `30` (Every 30 minutes).
4. Enter the **Script content**:
   ```bash
   php /www/wwwroot/yourdomain.com/sitemap_generator.php
   ```
   _(Make sure to change `yourdomain.com` to your actual folder name)_
5. Click **Add Task**.

### 2. cPanel

1. Search for **Cron Jobs** in the cPanel search bar under the _Advanced_ section.
2. Under **Add New Cron Job**:
   - **Common Settings:** `Twice per hour (0,30 * * * *)` or Custom `*/30 * * * *`
   - **Command:**
     ```bash
     /usr/local/bin/php /home/username/public_html/sitemap_generator.php >/dev/null 2>&1
     ```
     _(Replace `username` with your cPanel username, and `public_html` with your domain's folder if it's an addon domain)._
3. Click **Add New Cron Job**.

### 3. DirectAdmin

1. Head to **Advanced Features** -> **Cron Jobs**.
2. Click **Create Cron Job**.
3. Set the time to run every 30 minutes (`Minute: */30`).
4. Enter the **Command**:
   ```bash
   /usr/local/php81/bin/php /home/username/domains/yourdomain.com/public_html/sitemap_generator.php
   ```
   _(Path to PHP might differ, e.g., `/usr/local/bin/php`. Adjust `username` and `yourdomain.com` accordingly)._
5. Click **Create**.

### 4. Plesk

1. Go to **Websites & Domains** -> **Scheduled Tasks**.
2. Click **Add Task**.
3. Select **Task Type:** `Run a PHP script`.
4. Define the **Script path**:
   ```text
   httpdocs/sitemap_generator.php
   ```
   _(Depending on your root directory, it could also be `yourdomain.com/sitemap_generator.php`)._
5. Set **Run:** `Cron style` -> `*/30 * * * *`.
6. Click **OK** to save.

### 5. VPS / Dedicated Server (CLI via SSH)

If you manage your own server via SSH (Ubuntu/Debian/CentOS) and don't use a control panel:

1. Connect to your server via SSH.
2. Open the crontab editor:
   ```bash
   crontab -e
   ```
3. Add the following line at the bottom to run the generator every 30 minutes:
   ```bash
   */30 * * * * /usr/bin/php /path/to/your/website/sitemap_generator.php >/dev/null 2>&1
   ```
   _(Ensure you change `/path/to/your/website` to your actual absolute web root path, e.g., `/var/www/html/sitemap_generator.php`)_
4. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X` for Nano).\_
