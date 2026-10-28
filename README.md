# kappa-air.com

Kappa-Air, LLC: the home warranty side of the business. Static site on GitHub Pages, served from `docs/`.
It replaced the old WordPress site (GoDaddy hosting, WordPress 5.9, PHP 7.4) and keeps its page URLs.

Kappa-Air takes home warranty work, so the site speaks well of home warranty companies and explains how a warranty
claim works. There is no DIY section and no pricing: the warranty company sets coverage and prices.
(Retail customers belong at copperpeakair.com.)

## Editing

Three pages: home, `/home-warranty/` (how warranty service works, repairs, replacements, what isn't covered,
maintenance), and contact. The old WordPress addresses (`/ac-repair/`, `/heating-repair/`, `/ac-installation/`,
`/air-conditioning-and-heating-maintenance/`, `/indoor-air-quality/`, `/service-area/`) forward to the matching
section; the list is `REDIRECTS` in `tools/build.js`.

The warranty-process wording follows the published contracts (American Home Shield's Arizona contract, Old
Republic's Arizona plan, First American, 2-10): the warranty company picks the contractor, decides repair vs.
replace, and must approve work in advance; homeowners commonly pay for permits, code upgrades, modifications,
cranes, extra refrigerant, haul-away, and excluded items. Keep claims general ("depending on your plan").

- Pages: `content/pages/*.html` (front matter: `title`, `description`, `nav`)
- Header, nav, footer, styles, phone/email/address, city list: `tools/build.js` (the `SITE` object at the top)
- Logo and favicon: `static/`

Then run `node tools/build.js`, commit, and push. Never edit `docs/` by hand.

## Preview vs. live

While `site.config.json` has `"domain": ""`, the build targets the preview at https://tooshel.github.io/kappa-air/
(links get the `/kappa-air/` prefix, pages carry `noindex`, and there is no CNAME).

## Going live (when you're happy with the preview)

DNS for kappa-air.com is at GoDaddy (ns69/ns70.domaincontrol.com). Email is Google Workspace: **leave the MX
records alone.**

1. In GoDaddy DNS for kappa-air.com, **first** set the apex A records (`@`) to GitHub Pages, replacing the old
   GoDaddy hosting IP 208.109.27.101:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   (Do this before step 3; GitHub requests the certificate once, and if the apex isn't pointed yet the certificate
   only covers www.)
2. Set `www` to a CNAME pointing at `tooshel.github.io`.
3. Set `"domain": "www.kappa-air.com"` in `site.config.json`, run `node tools/build.js`, commit, and push. The
   build then writes `docs/CNAME`, drops the `noindex`, and uses root links.
4. In the repo's Settings → Pages, wait for the certificate, then tick **Enforce HTTPS**.
5. Cancel the old WordPress hosting once the new site has been up for a few days.
