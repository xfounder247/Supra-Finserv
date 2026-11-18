# Supra Finserv website

A responsive static 5-page website:
- `index.html` — Home
- `loans.html` — Loan categories
- `cibil.html` — Credit-report / CIBIL guidance
- `about.html` — About the consultancy
- `contact.html` — Contact and WhatsApp consultation form
- `assets/style.css` and `assets/site.js` — shared styling and interactions

## Before publishing
1. Confirm the business address, phone and email are correct.
2. Replace or add real business registration, DSA/lender association details only if applicable and verified.
3. Add a privacy policy and terms page if you collect or process personal information.
4. The consultation form is frontend-only: it opens WhatsApp with a prefilled message. It does not store submissions or send them automatically.
5. Google Fonts are loaded externally; the site still works with fallback fonts if unavailable.

## Deploy on Ubuntu VPS with Nginx
Upload the contents of this folder to `/var/www/supra-finserv` (not the ZIP itself), then configure Nginx. Example:

```nginx
server {
    listen 80;
    server_name YOUR_DOMAIN www.YOUR_DOMAIN;
    root /var/www/supra-finserv;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Save as `/etc/nginx/sites-available/supra-finserv`, then run:

```bash
sudo mkdir -p /var/www/supra-finserv
sudo chown -R $USER:$USER /var/www/supra-finserv
sudo ln -s /etc/nginx/sites-available/supra-finserv /etc/nginx/sites-enabled/supra-finserv
sudo nginx -t
sudo systemctl reload nginx
```

If the symlink already exists, do not create it again. For HTTPS, after DNS points to this VPS and port 80 is reachable, install Certbot and run:

```bash
sudo apt update
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d YOUR_DOMAIN -d www.YOUR_DOMAIN
```

## GitHub workflow
Create a repository, upload all files while preserving the `assets/` folder, then clone the repository on the VPS:

```bash
cd /var/www
sudo git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git supra-finserv
sudo chown -R www-data:www-data /var/www/supra-finserv
```

For updates:

```bash
cd /var/www/supra-finserv
sudo git pull
sudo nginx -t && sudo systemctl reload nginx
```

Do not commit passwords, API keys or private customer data.
