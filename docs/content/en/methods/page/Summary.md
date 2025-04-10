---
title: Summary
description: Returns the summary of the given page.
categories: []
keywords: []
params:
  functions_and_methods:
    returnType: template.HTML
    signatures: [PAGE.Summary]
---

<!-- Do not remove the manual summary divider below. -->
<!-- If you do, you will break its first literal usage on this page. -->

<!--more-->

You can define a [summary] manually, in front matter, or automatically. A manual summary takes precedence over a front matter summary, and a front matter summary takes precedence over an automatic summary.

To list the pages in a section with a summary beneath each link:

```go-html-template
{{ range .Pages }}
  <h2><a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a></h2>
  {{ .Summary }}
{{ end }}
```

Depending on content length and how you define the summary, the summary may be equivalent to the content itself. To determine whether the content length exceeds the summary length, use the [`Truncated`] method on a `Page` object. This is useful for conditionally rendering a “read more” link:

```go-html-template
{{ range .Pages }}
  <h2><a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a></h2>
  {{ .Summary }}
  {{ if .Truncated }}
    <a href="{{ .RelPermalink }}">Read more...</a>
  {{ end }}
{{ end }}
```

> [!note]
> The `Truncated` method returns `false` if you define the summary in front matter.

[`Truncated`]: /methods/page/truncated
[summary]: /content-management/summaries/
