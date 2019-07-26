---
title: Google Fonts in Hugo Themes
date: 2019-04-22
image: hugo.png
categories:
  - dev
---

In working on a few [Hugo](https://gohugo.io) themes, I wanted to make it relatively easy to specify custom Google Fonts inside the Hugo config file.

I'm not saying this is the best solution, but this is a solution.

In your `partials` folder:

```html
{{ if .Site.Params.google_fonts }}
  {{ $fonts := slice }}
  {{ range .Site.Params.google_fonts }}
    {{ $family := replace (index (.)  0) " " "+" }}
    {{ $weights := replace (index (.) 1) " " "" }}
    {{ $string := print $family ":" $weights }}
    {{ $fonts = $fonts | append $string }}
  {{ end }}
  {{ $url_part := (delimit $fonts "|") | safeHTMLAttr }}
  <link {{ printf "href=\"//fonts.googleapis.com/css?family=%s\"" $url_part | safeHTMLAttr }} rel="stylesheet">
{{ else}}
  <!-- specify a default in case custom config not present -->
  <link href="//fonts.googleapis.com/css?family=Roboto:300,400,700" rel="stylesheet">
{{ end }}
```

Then, we use that partial in our `head` somewhere.

```html
<head>
  {{ partial "google-fonts" . }}
</head>
```

Now, in our `config.toml`, we can do something fairly elegant like:

```toml
[params]
  google_fonts = [
    ["Fira Code", "400, 700"],
    ["Open Sans", "400, 400i, 700, 700i"]
  ]

  heading_font = "Fira Code"
  body_font = "Open Sans"
```

And then we can use those two variables in our SCSS if we want:

```sass
$font-heading: {{ $.Site.Params.heading_font | default "'Roboto', sans-serif" }};
$font-body: {{ $.Site.Params.body_font | default "'Roboto', sans-serif" }};

body {
  font-family: $font-body;
}

h1, h2, h3, h4, h5, h6 {
 font-family: $font-heading; 
}
```

[Gist](https://gist.github.com/jeremybise/a6afea2d4c7f9044180ffeb663a617cf)