# Meta and Link tags in header in HTML5

## Favicons

```html
<!-- For all browsers -->
<link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="favicon-16x16.png" />

<!-- For Google and Android -->
<link rel="icon" type="image/png" sizes="192x192" href="favicon-192x192.png" />
<link rel="icon" type="image/png" sizes="48x48" href="favicon-48x48.png" />
<link rel="manifest" href="site.webmanifest" />

<!-- For iPad -->
<link
  rel="apple-touch-icon"
  type="image/png"
  sizes="167x167"
  href="favicon-167x167.png"
/>

<!-- For iPhone -->
<link
  rel="apple-touch-icon"
  type="image/png"
  sizes="180x180"
  href="favicon-180x180.png"
/>

<!-- Old ICO -->
<link rel="shortcut icon" href="favicon.ico" />
```

## Twitter / X

```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@YourAccount" />
<meta name="twitter:creator" content="@YourAccount" />
<meta name="twitter:title" content="Title of your page" />
<meta name="twitter:url" content="URL of your page" />
<meta name="twitter:description" content="Your description here" />
<meta name="twitter:image:src" content="URL of image" />
```

## Facebook

```html
<meta property="og:title" content="ENTER PAGE TITLE" />
<meta property="og:type" content="website" /><!--Different values possible -->
<meta property="og:url" content="ENTER PAGE URL" />
<meta property="og:image" content="URL OF IMAGE" />
<meta property="og:image:width" content="1240" />
<meta property="og:image:height" content="650" />
<meta property="og:site_name" content="ENTER YOUR SITE NAME" />
<meta property="og:description" content="ENTER YOUR PAGE DESCRIPTION" />
<meta property="fb:app_id" content="ENTER YOUR FB APP ID" />

<meta property="og:see_also" content="URL to recommended page number 1" />
<meta property="og:see_also" content="URL to recommended page number 2" />
<!--UP TO 5 WEBSITE ADRESSES -->
```

## Search Engines

Some global brands use thumbnail in meta tag on their websites. Usually it has a dimensions of 150x150px and PNG format. This image may be used by browsers to display **in search results**.

```html
<meta name="thumbnail" content="path/to/image/thumb-150x150.png" />
```

Schema.org (microdata) in a JSON-LD format:

```html
<script type="application/ld+json">
  {
    "@context": "http://www.schema.org",
    "@type": "LocalBusiness",
    "name": "Ryan Weber Ltd",
    "url": "https://ryan-weber.com",
    "logo": "https://ryan-weber.com/ryan-weber-logo-192px.png",
    "image": "https://ryan-weber.com/ryan-weber-logo-192px.png",
    "description": "Ryan Weber Ltd delivers bespoke websites, web applications, mobile apps and AI integrations to Your Business.",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "71-75 Shelton Street",
      "addressLocality": "London",
      "postalCode": "WC2H 9JQ",
      "addressCountry": "United Kingdom"
    },
    "hasMap": "https://maps.app.goo.gl/cMVQeXsWnpsaLnDc7",
    "openingHours": "Mo 01:00-01:00 Tu 01:00-01:00 We 01:00-01:00 Th 01:00-01:00 Fr 01:00-01:00 Sa 01:00-01:00 Su 01:00-01:00"
  }
</script>
```
