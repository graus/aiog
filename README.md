# AI & Open Government Workshop (AI&OG)

Website for the AI & Open Government Workshop at ICAIL 2026.

**Using AI and LLMs to Advance Open Government and Public Disclosure Laws Worldwide**

## Live Site

https://aiog.net

## Local Development

Simply open `index.html` in a browser, or use a local server:

```bash
python -m http.server 8000
```

Then visit http://localhost:8000

## Deployment

This site is deployed via GitHub Pages. Any push to the `main` branch will automatically update the live site.

## DNS Configuration

To connect your custom domain (aiog.net), add these DNS records:

**Option A: Apex domain**
```
A     @     185.199.108.153
A     @     185.199.109.153
A     @     185.199.110.153
A     @     185.199.111.153
```

**Option B: www subdomain (optional)**
```
CNAME www   <username>.github.io
```

See [GitHub Pages custom domain docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) for more details.
