# Affordable Mobile TV Repair Tech Demo
This is a design and technology demonstration, not a live business website. All contact information, reviews, business claims, and social media links are fictitious placeholders. The business name "Affordable Mobile TV Repair" is used for demonstration purposes only.

> [!IMPORTANT]   
> This entire repo, except the software licenses and this notice, was generated using Claude `opus-4.6`.


## What This Is
A single-page marketing website for a mobile TV repair company, built as a proof of concept to show what can be done with modern web technologies and zero build tooling. The site is fully functional, mobile-responsive, and could be edited into a production website with minimal effort.


## Technical Implementation
The entire website is a single `index.html` file. There is no build step, no bundler, no package.json, no node_modules. You open the file in a browser and it works. This matters because it means:

- Deployment is trivially simple (copy one file)
- There is nothing to break, update, or maintain on the tooling side
- Any developer or even a motivated non-developer can edit it
- It will still work in 10 years


### Vue.js
The site uses Vue 3 via CDN (`unpkg.com/vue@3/dist/vue.global.prod.js`). This is the only external dependency. Vue was chosen over vanilla JS because it provides reactive data binding and component-like structure without requiring a build step. The Composition API (`setup()`) is used throughout.

All content (services, brands, reviews, steps, features) is defined as plain arrays in the `setup()` function. To change the site's content, you edit these arrays. No hunting through HTML. This is the main architectural benefit of using Vue here: separation of content from presentation, without the overhead of a CMS or a build pipeline.

The `v-for` directive handles all repeated elements (service cards, brand chips, review cards, etc.), so adding a new brand or removing a service is a one-line change.


### CSS Architecture
All styling is in a single `<style>` block using CSS custom properties (variables) for theming. The color palette, border radii, shadows, and font stacks are all defined in `:root` and referenced throughout. To rebrand the site, you change maybe 10 variable values.

The layout uses CSS Grid and Flexbox exclusively. No float hacks, no CSS frameworks, no utility class libraries. Media queries at 840px and 600px handle the tablet and mobile breakpoints respectively.

Scroll-triggered fade-in animations are handled by a simple scroll listener that adds a `.visible` class to `.fade-up` elements when they enter the viewport. This is done in vanilla JS inside Vue's `onMounted` hook rather than using an Intersection Observer, which would be slightly more efficient but adds complexity that isn't justified for a page this size.


### Typography
The site uses system font stacks rather than loading web fonts:
- Display/headings: `'Segoe UI Black', 'Arial Black', 'Trebuchet MS', sans-serif`
- Body text: `Georgia, 'Times New Roman', serif`
- Monospace (not heavily used): `'Courier New', Courier, monospace`

This means zero additional network requests for fonts, which improves load time and avoids the flash of unstyled text (FOUT) problem entirely. The tradeoff is less typographic control across platforms, but for a local business site the pragmatism wins.


### Icons
All icons are inline SVGs directly in the HTML/Vue templates. No icon font, no icon library, no additional HTTP requests. Each icon is a few hundred bytes. This is the most efficient approach for a site with fewer than ~50 icons.


### The Form
The contact form uses `@submit.prevent` to suppress the default form submission. In this demo it just toggles a "Quote Requested!" confirmation state for 4 seconds and resets. In production, you would replace the `submitForm` function with an actual submission handler. Options include:
- A third-party form service like Formspree, Basin, or Getform (no backend needed)
- An AWS Lambda function behind API Gateway (if you're already on AWS/S3)
- A simple POST to whatever backend the business ends up using

The form does not use `<form action="...">` with a server endpoint because the site is designed for static hosting.


## Hosting
This site is designed to be hosted as a static file. Here are three viable options with cost estimates. All assume the `index.html` file is approximately 40 KB.


### GitHub Pages - current hosting
Cost: Free.

GitHub Pages provides free static hosting for public repositories. You push the file, enable Pages in the repo settings, and you get a URL. Custom domains are supported at no charge and GitHub automatically provisions an SSL certificate via Let's Encrypt.

The soft bandwidth limit is 100 GB/month. At 40 KB per page load, that is approximately 2,500,000 page views per month before you would theoretically approach the limit. GitHub does not hard-cut sites that exceed this; they send a polite email.

GitHub Pages is not officially intended for commercial websites, but a simple demo site like this is well within the norm of what people host there.

To deploy:
1. Create a GitHub repository
2. Push `index.html` to the root of the `main` branch
3. Go to Settings, then Pages, then select "Deploy from a branch" with `main` and `/ (root)`
4. Wait 30-60 seconds for the URL to go live


### AWS S3 + CloudFront
Cost: Approximately $0.50-1.50/month for a low-traffic local business site.

S3 static website hosting costs $0.023/GB for storage (the file is 40 KB, so essentially $0.00) plus $0.0004 per 1,000 GET requests. CloudFront (CDN) in front of S3 adds $0.085/GB for data transfer in North America.

Rough estimates by traffic level:

| Monthly page views | Approximate cost |
|--------------------|-----------------|
| 10,000             | ~$0.50          |
| 100,000            | ~$1.00          |
| 1,000,000          | ~$5.00          |
| 10,000,000         | ~$45.00         |

These are rough estimates. Actual costs depend on request patterns, cache hit ratios, and whether you serve the file directly from S3 (cheaper but slower, no HTTPS on custom domain) or through CloudFront (faster, HTTPS, slightly more expensive).

To deploy:
1. Create an S3 bucket
2. Enable static website hosting in the bucket properties
3. Upload `index.html`
4. Optionally create a CloudFront distribution pointing to the S3 bucket
5. Point your custom domain's DNS to CloudFront (or to the S3 website endpoint if not using CloudFront)


### Cloudflare Pages
Cost: Free.

Cloudflare Pages offers free static hosting with unlimited bandwidth on the free tier. It connects to a GitHub or GitLab repository and auto-deploys on push. Custom domains are free and SSL is automatic. This is arguably the best option if you want the reliability of a CDN with zero cost and don't mind the Cloudflare ecosystem.


## Files
```bash
CHANGE_LICENSE  # the software license applicable starting four years from the commit timestamp
index.html      # the entire website
LICENSE         # the software license applicable from the commit timestamp and four years thereafter
README.md       # this documentation
```


## Editing the Content
To change the business name, phone number, email, or any other content, open `index.html` in a text editor and search for the text you want to change. Most repeated content (services, brands, reviews) is in the JavaScript `setup()` function near the bottom of the file as plain arrays of objects. The hero section, navigation, and contact info are in the HTML template above.

The brand list is a simple array of strings:
```js
const brands = [
  'Samsung', 'LG', 'Sony', 'TCL', ...
];
```
Adding a brand is adding a string to the array. Removing one is deleting it. The grid layout adapts automatically.


## License
This project is licensed under the [Business Source License 1.1](./LICENSE) (BUSL-1.1). You may copy, modify, create derivative works, and redistribute the code, but production use is not permitted without a commercial license from the author.

Each commit is treated as a distinct version of the Licensed Work. Four years after a commit's timestamp, that commit's code automatically relicenses under the [MIT License](./CHANGE_LICENSE). Newer commits remain under the BUSL until their own four-year window expires.

See [LICENSE](./LICENSE) and [CHANGE_LICENSE](./CHANGE_LICENSE) for the full legal text.


---
> **_Notice_**
> This repo contains assets created in collaboration with a large language model, machine learning algorithm, or weak artificial intelligence (AI). This notice is required in some countries.