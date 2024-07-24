# AI-CONTROL - CURRENT STATUS: WIP
# Multi-Level Approach to Managing AI Crawler Behaviour and Content Protection

## Executive Summary

This document proposes a comprehensive, multi-layered strategy to protect website content from unauthorized use in AI training, particularly by Large Language Models (LLMs). The approach leverages existing web standards and introduces new methods to communicate content usage restrictions effectively.

## Objectives

1. Establish new standards for the Robots Exclusion Protocol (RFC 9309) to control AI-oriented crawlers.
2. Implement multiple further layers of protection to ensure content creators' rights are respected.
3. Utilize existing directives where possible and introduce new ones where necessary.
4. Encourage widespread adoption through major platforms and content management systems.

## Proposed Multi-Level Approach

### 1. Enhanced Robots.txt Directives

**Standard Implementation:**
```plaintext
User-agent: *
Disallow: /private/

User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

# Link to licensing information
Sitemap: https://www.example.com/sitemap.xml
License-Info: https://www.example.com/license-info.json
```

**User Agent-Specific Implementation:**
Serve different robots.txt files based on user agents, potentially excluding all AI crawlers.

**References:**
- nixCraft: How to block AI Crawler Bots using robots.txt file[4]
- Raptive Support: How to manually block common AI crawlers[4]

### 2. Licensing Information in Robots.txt

Reference a JSON-LD or XML file containing detailed licensing information:

**Example JSON-LD Licensing Information:**
```json
{
  "@context": "https://schema.org",
  "@type": "CreativeWork",
  "license": {
    "@type": "CreativeWork",
    "name": "Custom AI Usage License",
    "url": "https://www.example.com/ai-usage-license"
  },
  "usageInfo": {
    "aiTraining": "disallowed",
    "summarization": "allowed",
    "reproduction": "disallowed"
  }
}
```

### 3. HTTP Headers

**X-Robots-Tag:**
```http
X-Robots-Tag: noindex, noarchive, nosnippet, noai
```

**Cache-Control:**
```http
Cache-Control: no-store, no-cache, must-revalidate
```

**Custom Legal Notice:**
```http
X-Legal-Notice: Unauthorized use of this content for AI training is prohibited. See /terms-of-service for details.
```

**License Information:**
```http
X-License-Info: CC-BY-NC-4.0
```

**References:**
- Loganix: What Is an X-Robots-Tag?[6]

### 4. HTML Meta Tags

**Example Meta Tags:**
```html
<head>
  <meta name="robots" content="noindex, noarchive, nosnippet, noai">
  <meta name="license" content="https://www.example.com/license-info">
</head>
```

### 5. XML Sitemaps

Create a specific sitemap for restricted URLs:

**Example Restricted XML Sitemap:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.example.com/restricted-page</loc>
    <lastmod>2024-07-24</lastmod>
    <changefreq>never</changefreq>
    <priority>0.0</priority>
  </url>
</urlset>
```

### 6. Terms of Service and Legal Notices

**Update ToS to Include:**
```plaintext
Prohibited Uses: The use of any content from this website for the purpose of training, developing, or operating artificial intelligence (AI) models, machine learning (ML) systems, or any other automated systems is strictly prohibited.
```

**Display Visible Notice:**
```html
<footer>
  <p>© 2024 [Your Name/Company]. All Rights Reserved. Unauthorized use of this content for AI training or data mining is strictly prohibited. See our <a href="/terms-of-service">Terms of Service</a> for more details.</p>
</footer>
```

**References:**
- DoubleCloud: AI Terms and Conditions[5]
- Hugh Stephens Blog: DMCA Copyright Infringement? The Perils of Relying on AI[5]

### 7. Content Watermarking

Implement invisible watermarks in text and images to track unauthorized use.

**References:**
- Digital Guardian: How to Watermark Your Data[6]
- arxiv.org: DiffusionShield: A Watermark for Diffusion Models[6]

### 8. DMCA Notices

Encourage institutions to establish clear processes for handling DMCA notices related to AI training data.

**References:**
- Frost Brown Todd: DMCA Notices for AI Training Data[5]

## Implementation Recommendations

1. **Platform Integration:** Encourage major platforms (e.g., Cloudflare, WordPress, Google Search Console) to implement these controls in their user interfaces.
2. **Standardization:** Work with industry bodies to standardize these approaches across different platforms and services.
3. **Education:** Provide resources and guidance for content creators on implementing these protections.
4. **Legal Framework:** Advocate for clear legal guidelines regarding the use of copyrighted material in AI training.
5. **Monitoring and Enforcement:** Develop tools and services to help content creators monitor for unauthorized use of their content in AI training.

## Conclusion

This multi-level approach provides a comprehensive strategy for protecting content from unauthorized use in AI training. By leveraging existing web standards and introducing new methods, content creators can effectively communicate their usage restrictions and maintain control over their intellectual property in the age of AI.

Sources
[1] https://datatracker.ietf.org/group/aicontrolws/about/
[3] https://foundation.mozilla.org/en/blog/ai-training-can-undermine-the-open-web-this-team-is-thinking-through-solutions/
[4] https://de.wikipedia.org/wiki/Robots_Exclusion_Standard
[5] https://commonpaper.com/standards/cloud-service-agreement/train-ai/
[6] https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/Generative_AI_Models.pdf?__blob=publicationFile&v=4
