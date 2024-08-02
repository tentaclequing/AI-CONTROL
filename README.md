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

### Layers

Crawling directives alone typically rely on the voluntary compliance of web crawlers, which is not always guaranteed. Therefore, it is essential to implement additional methods to safeguard intellectual property effectively.

1. Crawling and Indexation Directives
2. Licensing and Legal Information
3. Copyright Protection and Monitoring
4. User-Friendly Implementation and Accessibility


### 1. Enhanced Robots.txt Directives

**Standard Implementation:**
The standard robots.txt file allows website administrators to specify which parts of their site should be allowed or disallowed for crawling. This requires knowledge of the exact user agents used by generative AI bots to scrape websites.

```plaintext
# Crawling directives for all user agents unless specified separately in this file
User-agent: *
Disallow: /private/

# Disallowing access for specific user agents associated with generative AI
User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

# Link to licensing information
Sitemap: https://www.example.com/sitemap.xml
License-Info: https://www.example.com/license-info.json
```

Maintaining the robots.txt file is straightforward and quick; however, it comes with certain disadvantages:
1. Exposure of Sensitive Information: The file is publicly accessible and readable by humans, which could inadvertently reveal paths containing sensitive information. This might encourage malicious activities such as DDoS attacks or other cybersecurity threats targeting those paths.
2. Non-Compliance by Bots: Not all bots obey robots.txt directives. There have been instances where bots, such as Perplexity AI, have crawled websites regardless of the directives. Therefore, it is strongly recommended to implement additional layers of protection.


**User Agent-Specific Implementation:**
Dynamic serving of different robots.txt files based on user agents, potentially excluding all AI crawlers requires server-side handling but adds an additional layer. For instance, Reddit recently implemented a strategy to block all search engines except Google by serving a different robots.txt files for Googlebot. Other user agents - including all other search engines - see their classic robots.txt file, disallowing all access and referencing their public content policy to inform about how they allow or disallow content to be accessed and used (Ryan Siddle, Merj Blog on Reddit's robots.txt Cloaking Strategy [1]).

Dynamic user-agent-based serving of robots.txt files requires server-side handling, which adds complexity. If a domain does not want to expose their strategy of which user agents they wish to block, there are some advantages. 

**Example robots.txt cloaking strategy:**
1. Identify user agents or IP ranges of bots associated with generative AI. 
2. Serve these bots a robots.txt with strict exclusion rules, protecting content you do not wish to be scraped.
3. For all other user agents or IP addresses, serve a "vanilla" version of your robots.txt, listing all rules for benevolent bots like search engines.

To cloak a robots.txt file for specific user agents, website administrators using Cloudflare can leverage Cloudflare Workers. If dynamic serving relies on user agents, validate user-agent strings to prevent spoofing.

### 2. Licensing Information in Robots.txt

Combining licensing information with other directives in robots.txt adds an additional layer of protection and complements other methods like HTTP headers and meta tags, creating a multi-faceted approach to safeguarding a website's content.

Instead of going with Reddit's approach of adding licensing information to human-readable pages, we suggest adding this information to an machine-readable format like JSON-LD or XML. This file can then be referenced in robots.txt, providing a clear and accessible way to communicate the terms under which a website's content can be used.
We suggest establishing a standard for licensing JSON-LD or XML files.

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

Advantages:
1. Legal Clarity: While robots.txt directives are not legally binding, adding licensing information can serve as a clear notice of a website's content usage rights and strengthen website owners' position in case of disputes over unauthorized use of their content.
2. Ease of Implementation: Updating robots.txt to include a link to a licensing information file is straightforward and does not require significant technical changes.


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
[1] https://merj.com/blog/investigating-reddits-robots-txt-cloaking-strategy

[3] https://foundation.mozilla.org/en/blog/ai-training-can-undermine-the-open-web-this-team-is-thinking-through-solutions/
[4] https://de.wikipedia.org/wiki/Robots_Exclusion_Standard
[5] https://commonpaper.com/standards/cloud-service-agreement/train-ai/
[6] https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/Generative_AI_Models.pdf?__blob=publicationFile&v=4
