# workflows/web-research.md

Browser-based research playbook. How web pages, articles, and external references get into the system without burning context or shipping shallow notes.

## Tool selection, in order

The assistant has three broad ways to pull web content. Pick the right one before fetching.

1. **Dedicated MCP for the app.** If the source lives in a tool with a connector (a CRM, an issue tracker, a notes app, a docs platform), use the connector. API-backed access is fast, returns structured data, and avoids rendering quirks.
2. **Browser MCP.** For public web pages and articles, use the browser MCP. Renders JavaScript, sees the page as a human would, can extract clean body text in one shot.
3. **Simple HTTP fetch.** For static pages where you know the structure, a single fetch can be cheaper than spinning the browser.

Tool choice is a system behavior. Choosing the wrong tool is its own failure mode.

## One-shot extraction

The default browser extraction is one JavaScript call against the page, targeting the content node, returning innerText:

```
document.querySelector('article, main, [role="main"]').innerText
```

The selector falls through: try the semantic element first, then `main`, then a role-tagged region, then the page body. This is the difference between a clean read and a soup of navigation and ads.

Never chunk-slice a page. Never ask the human to "continue?" between chunks. Never wait on a `document_idle` event that the page may not emit. Pull the content in one shape, work with it.

## Site-type playbook

Different sites need different handling.

**News and blogs.** The semantic `article` selector usually works. If not, target the publication's known content class.

**Long-form magazines.** Often multi-page. Check for a "next page" link and follow it; concatenate the extracted text before writing the wiki entry.

**Academic journals.** Most have an HTML view in addition to the PDF. The HTML view is faster to extract from. The PDF is canonical when verifying quotes.

**Substack and similar.** Articles render server-side. The browser MCP gets the full text in one call. If the post is paid-only, the extraction returns the preview only. Flag and route to the human.

**Documentation sites.** Often built on the same generator family (Docusaurus, MkDocs, etc.). The content lives in a known node class. A site-specific selector beats the default.

**Single-page apps.** The page does not render content until JavaScript runs. The browser MCP handles this; an HTTP fetch returns a shell. Use the browser.

## Verification

After extraction, the first check is: did the right content load?

- The first 200 characters should be the article body, not navigation.
- The text should be in the language and on the topic expected.
- For long articles, the closing paragraph should actually be the closing paragraph, not a "you may also like" recommendation block.

If verification fails, try a different selector before re-fetching. Most failures are extraction quality, not network errors.

## Blocked or paywalled content

Some domains return blank, return a login page, or are explicitly blocked. The escalation path:

1. Note the block. Do not retry.
2. Look for the same content on an unblocked surface (the publication's syndication partner, an open archive, an author's personal site).
3. If no unblocked source exists, surface the block to the human and ask them to provide the content if they have access.

Never fabricate content to fill a gap. The wiki is only valuable if every entry is verifiable.

## Multi-source synthesis

For a research task that spans several articles, the workflow is:

1. List the candidate URLs up front.
2. Extract each, verify each, write a brief note to a working file.
3. Only after every source is captured, write the synthesis.
4. The synthesis cites each source by URL, not by paraphrase.

Synthesizing as you read produces drift. Capture first, synthesize second.

## When the source ends up in the wiki

Web articles that warrant a permanent entry follow the schema in `wiki/schema.md`. Note the access date in the frontmatter, since web content drifts:

```yaml
---
url: https://example.com/article
accessed: 2026-06-04
---
```

A wiki entry for a web article is the same shape as one for a paper. The fact that it came from a browser does not change the schema.

## What never counts as web research

- Chat with a chatbot about a topic, then citing that chat as the source.
- Pulling a single search result snippet and treating it as the article.
- Extracting from a cached version when the live page differs.
- Skipping verification because "the page looked right."

## See also

- `research.md` for the broader ingestion pipeline
- `verification.md` for the fact-check layer
- `wiki/schema.md` for the entry format
