---
title: InSection
description: Reports whether the given page is in the given section.
categories: []
keywords: []
params:
  functions_and_methods:
    returnType: bool
    signatures: [PAGE.InSection SECTION]
---

{{% glossary-term section %}}

The `InSection` method on a `Page` object reports whether the given page is in the given section. Note that the method returns `true` when comparing a page to a sibling.

With this content structure:

```text
content/
├── auctions/
│   ├── 2023-11/
│   │   ├── _index.md
│   │   ├── auction-1.md
│   │   └── auction-2.md
│   ├── 2023-12/
│   │   ├── _index.md
│   │   ├── auction-3.md
│   │   └── auction-4.md
│   ├── _index.md
│   ├── bidding.md
│   └── payment.md
└── _index.md
```

When rendering the "auction-1" page:

```go-html-template
{{ with .Site.GetPage "/" }}
  {{ $.InSection . }} → false
{{ end }}

{{ with .Site.GetPage "/auctions" }}
  {{ $.InSection . }} → false
{{ end }}

{{ with .Site.GetPage "/auctions/2023-11" }}
  {{ $.InSection . }} → true
{{ end }}

{{ with .Site.GetPage "/auctions/2023-11/auction-2" }}
  {{ $.InSection . }} → true
{{ end }}
```

In the examples above we are coding defensively using the [`with`] statement, returning nothing if the page does not exist. By adding an [`else`] clause we can do some error reporting:

```go-html-template
{{ $path := "/auctions/2023-11" }}
{{ with .Site.GetPage $path }}
  {{ $.InSection . }} → true
{{ else }}
  {{ errorf "Unable to find the section with path %s" $path }}
{{ end }}
  ```

## Understanding context

Inside of the `with` block, the [context](g) (the dot) is the section `Page` object, not the `Page` object passed into the template. If we were to use this syntax:

```go-html-template
{{ with .Site.GetPage "/auctions" }}
  {{ .InSection . }} → true
{{ end }}
```

The result would be wrong when rendering the "auction-1" page because we are comparing the section page to itself.

> [!note]
> Use the `$` to get the context passed into the template.

```go-html-template
{{ with .Site.GetPage "/auctions" }}
  {{ $.InSection . }} → true
{{ end }}
```

> [!note]
> Gaining a thorough understanding of context is critical for anyone writing template code.

[`else`]: /functions/go-template/else/
[`with`]: /functions/go-template/with/
