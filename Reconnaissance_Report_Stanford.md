# Ethical Hacking Reconnaissance Report - Stanford University

## Part A: Target Selection
- **Website Name**: Stanford University
- **URL**: `https://stanford.edu`
- **Reason for choosing the target**: A globally recognized educational institution. Chosen as a publicly accessible, safe target for passive reconnaissance under strict non-intrusive constraints.

## Part B: WHOIS Lookup
*(Data gathered via EDUCAUSE WHOIS database)*

- **Domain Name**: STANFORD.EDU
- **Registrar**: EDUCAUSE
- **Registration Date**: 04-Oct-1985
- **Expiry Date**: 31-Jul-2028
- **Name Servers**:
  - AVALLONE.STANFORD.EDU
  - ATALANTE.STANFORD.EDU
  - ARGUS.STANFORD.EDU
  - NS7.DNSMADEEASY.COM
  - NS6.DNSMADEEASY.COM
  - NS5.DNSMADEEASY.COM
- **Domain Status**: Active

### WHOIS Output Evidence
```text
Domain Name: STANFORD.EDU
Registrant: Stanford University
Domain record activated:    04-Oct-1985
Domain expires:             31-Jul-2028
```

## Part C: DNS Enumeration
*(Data gathered via DNS lookup)*

- **A Record**: `171.67.215.200`
- **MX Record**: 
  - `mxb-00000d07.gslb.pphosted.com` (Preference: 10)
  - `mxa-00000d07.gslb.pphosted.com` (Preference: 10)
- **NS Record**: `ns7.dnsmadeeasy.com`, `atalante.stanford.edu`, `ns5.dnsmadeeasy.com`, `avallone.stanford.edu`, `ns6.dnsmadeeasy.com`, `argus.stanford.edu`

### DNS Output Evidence
```text
Name                                           Type   TTL   Section    IPAddress                                
----                                           ----   ---   -------    ---------                                
stanford.edu                                   A      1800  Answer     171.67.215.200    
```

## Part D: Website Technology Identification
- **Framework**: Next.js (Identified via `X-Powered-By` and `X-Nextjs-*` headers)
- **Web Server/Hosting**: Netlify (Identified via `Server` and `X-Nf-Request-Id` headers)
- **Email Service**: Proofpoint (Identified via MX record `.pphosted.com`)
- **DNS Service**: DNSMadeEasy (alongside custom Stanford nameservers)

**Summary**: The Stanford website is a modern web application built using the Next.js framework and hosted on Netlify's Edge infrastructure. Email security and routing are handled externally by Proofpoint.

## Part E: HTTP Security Headers

| Header | Present (Yes/No) | Purpose |
| :--- | :--- | :--- |
| **Content-Security-Policy (CSP)** | Yes | Highly restrictive CSP utilizing nonces (`nonce-11dU...`) to prevent Cross-Site Scripting (XSS). |
| **X-Frame-Options** | Yes | Prevents clickjacking. The site uses the strictest setting: `DENY`. |
| **X-Content-Type-Options** | Yes | Prevents MIME-sniffing. Set to `nosniff`. |
| **Strict-Transport-Security (HSTS)** | Yes | Enforces secure HTTPS connections. Set to `max-age=31536000` (1 year). |
| **Referrer-Policy** | Yes | Controls referrer information. Set to `origin-when-cross-origin`. |
| **Permissions-Policy** | Yes | Restricts browser features (camera, microphone, geolocation) to mitigate risk. |

### HTTP Headers Evidence
```text
Content-Security-Policy: default-src 'self'; script-src 'nonce-...
Permissions-Policy: vibrate=(), geolocation=(self), midi=(), notifications=(), push=(), microphone=(), camera=...
Referrer-Policy: origin-when-cross-origin                                                                     
Strict-Transport-Security: max-age=31536000                                                                             
X-Content-Type-Options: nosniff                                                                                      
X-Frame-Options: DENY 
Server: Netlify                                                                                      
X-Powered-By: Next.js 
```

## Part F: Robots.txt & Sitemap Analysis

- **Does the website have a robots.txt file?** Yes. (`https://www.stanford.edu/robots.txt`).
- **Does it have a sitemap?** Yes. The `robots.txt` file points directly to it (`https://www.stanford.edu/sitemap.xml`).
- **What information can be learned from these files?**
  - Search engines are allowed to crawl the main site (`Allow: /`).
  - Restricted paths include component files, testing areas, and administrative tools: `/global-components/`, `/test/`, `/editor/`, `/search`, `/storybook/`, and `/examples/`.
  - The presence of `/storybook/` further confirms a modern frontend development stack (likely React/Next.js) using Storybook for UI component isolation.

### Robots.txt Evidence
```text
User-Agent: *
Allow: /
Disallow: /global-components/
Disallow: /test/
Disallow: /editor/
Disallow: /search
Disallow: /storybook/
Disallow: /examples/

Sitemap: https://www.stanford.edu/sitemap.xml
```
