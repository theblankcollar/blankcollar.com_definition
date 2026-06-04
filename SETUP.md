# Domain & hosting setup

This documents how the domains are wired up.

## Architecture

| URL | Serves | Hosted on |
| --- | --- | --- |
| `blankcollar.com` (apex) | Medium blog | Medium |
| `www.blankcollar.com` | Medium blog | Medium |
| `definition.blankcollar.com` | this definition page | Vercel |

The definition page links to the blog ("Read the blog →"). The blog links back to the
definition page through Medium's publication navigation.

DNS is managed **in Vercel**.

---

## One-time setup steps

Do these in order — stand up the subdomain **before** moving `www`/apex so the
definition page is never offline.

### 1. Put the definition site on the subdomain (Vercel)

1. Vercel → the `blankcollar` project → **Settings → Domains**.
2. **Add** `definition.blankcollar.com`. DNS is on Vercel, so it auto-creates the record
   and verifies.
3. Confirm `https://definition.blankcollar.com` loads this page.

### 2. Release `www` + apex from the Vercel project

4. Same **Settings → Domains** screen → **remove** `www.blankcollar.com` and
   `blankcollar.com` from the project, so Vercel stops serving them and stops forcing
   its own DNS records.

### 3. Repoint DNS to Medium (Vercel DNS)

5. Vercel → **Domains** (account level) → `blankcollar.com` → **manage DNS records**.
6. **Delete** the existing records that route apex/www to Vercel (typically
   `A @ → 76.76.21.21` and `CNAME www → cname.vercel-dns.com`).
   **Keep** the `definition` record and any **MX/TXT** (email/verification) records.
7. **Add** these four A records:

   | Type | Host | Value |
   | --- | --- | --- |
   | A | `@` | `162.159.153.4` |
   | A | `@` | `162.159.152.4` |
   | A | `www` | `162.159.153.4` |
   | A | `www` | `162.159.152.4` |

### 4. Connect the domain in Medium

8. Medium → publication/profile → **Settings → Custom domain → Get started**.
9. Enter `www.blankcollar.com` (Medium redirects the apex to it).
   - Heads up: Medium charges a **one-time ~$50 fee**.
   - Verification/propagation can take **up to ~72 hours**.

### 5. Add the blog → definition link (Medium)

10. In the Medium publication's **navigation / menu settings**, add a link labeled e.g.
    **"Definition"** → `https://definition.blankcollar.com`.

---

## Verifying it worked

```bash
# Blog (apex + www) should resolve to Medium's IPs:
dig www.blankcollar.com +short      # 162.159.153.4 / 162.159.152.4
dig blankcollar.com +short          # 162.159.153.4 / 162.159.152.4

# Definition page should resolve to Vercel:
dig definition.blankcollar.com +short
```

End state: `blankcollar.com` and `www.blankcollar.com` load the Medium blog;
`definition.blankcollar.com` loads this page; both directions cross-link.
