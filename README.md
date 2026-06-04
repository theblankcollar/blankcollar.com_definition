# blankcollar.com — definition page

A single-page dictionary-style definition of the term **blank collar**.

Static HTML/CSS. No build step. Auto-deployed to Vercel on push to `main`.

## Where this lives

This page is served at **`definition.blankcollar.com`** (Vercel). The apex
`blankcollar.com` and `www.blankcollar.com` point to the **Medium blog**. The page
links out to the blog ("Read the blog →"); the blog links back here via Medium's
publication navigation.

See [`SETUP.md`](./SETUP.md) for the DNS + Medium custom-domain setup.

## Local preview

Open `index.html` in a browser, or run a quick local server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy

Connected to Vercel — pushes to `main` deploy automatically.
