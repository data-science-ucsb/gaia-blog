+++
title = "In praise of the homelab"
date = 2023-07-12
description = "🌱"
+++

> Few of us understood it at the time, but none of the things that we’d go on to share would belong to us anymore.... Our attention, our activities, our locations, our desires—everything about us that we revealed, knowingly or not, was being surveilled and sold in secret.

– Edward Snowden, *Permanent Record*

If you haven't lately, read through a few [terms of service](https://tosdr.org/) and privacy policies online. If you're like me, you'll probably come back horrified.

I realized we don't get to *really* choose what to share and whom to share it with. So I wiped (most of) my iCloud Keychain and Google Drive [^1].

Yes, my friends looked at me funny when I told them.

I'm not a hermit, but I've decided to take back control of my data — passwords, calendars, and notes, for instance.

My solution? The homelab.

A homelab can be as simple as an old laptop or a Raspberry Pi. Mine started on a Raspberry Pi 3 and has since snowballed into an AMD Ryzen 5 build.

Now, I self-host passwords, a VPN, an RSS reader, a git server, and even [jadeocr](https://next.jadeocr.com/).



**You should host your own services, too!** Catalog where your most important digital data lives. Then [find](https://github.com/awesome-selfhosted/awesome-selfhosted) a self-hosted alternative you like and migrate.

Now, here comes the involved bit:

### A very brief guide to self-hosting

If you're less sysadmin-inclined, [Yunohost](https://yunohost.org/#/) seems to be a good bet to manage everything (although I haven't used it myself).

I use Ubuntu Server, Docker, and the NGINX reverse proxy [^2].

My services have NGINX config files that look roughly like this. I write them in `/etc/nginx/sites-available` and symlink them to `/etc/nginx/sites-enabled` (then restart NGINX).

```
# /etc/nginx/sites-available/reader.conf
server {
	listen 443 ssl;

	# uncomment after creating certs
	# ssl_certificate /path/to/fullchain.pem;
	# ssl_certificate_key /path/to/privkey.pem;

	server_name reader.tanaybiradar.com;

	location / {
		proxy_pass http://localhost:1300/;
	}
}
```

I use [Certbot](https://certbot.eff.org/) to manage HTTPS certificates (you **really** want HTTPS on all your services) [^3]. Then I forwarded port 443 (SSL) from my router to my server.

The last step was using Cloudflare to set up proxied DNS records for my domains [^4]. If you're using Cloudflare, make sure set the SSL/TLS encryption mode to "full".

That's it! The upfront effort is quite high, but adding additional services takes just a few minutes once you're used to the process.

That's how you claim a slice of the internet and make it yours. Use services on your own terms and choose what the world gets to see.

And you'll feel awesome every time you visit your own domain.

> "Here are the photos from the trip! https://photos.yourname.com/2023-07-san-francisco"

– You, probably

Start a homelab! You'll be better for it.

---

[^1]: I'm not totally sans-Google. I have some files that live on Google Drive, but I encrypt whatever I can with [rclone](https://rclone.org/) and only write non-sensitive data in Google Docs.

[^2]: A [reverse proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/) just directs incoming requests on the same server to their appropriate destinations.

[^3]: Make sure to have your domain's DNS records and NGINX `server_name` set correctly first. Uncomment lines in the config file and restart NGINX.

[^4]: to hide your home's IP address from `nslookup`
