# How to game the FAIR evaluator 😈

This guide will help you build an application that follows the FAIR principles and pass most tests of the [FAIR Evaluator](https://fairsharing.github.io/FAIR-Evaluator-FrontEnd/).

To make the explanation easier we will suppose you want to build a website which catalogs resources (e.g. datasets). 

## 🔗 Use a HTTP(S) URI

Each resource should be accessible at a given URL using the protocol `http://` or `https://`

> e.g. https://mywebsite.org/resource/my-resource

Ideally you should use a persistent identifier for your resources.

Redirect the request from your w3id.org namespace to your website URL.

> e.g. https://w3id.org/mywebsite/resource/my-resource

## 🤖 Send machine readable metadata

Each page should include a snippet of JSON-LD describing the resource presented on this page.

Add the JSON-LD of the resource in a `<script>` tag in the HTML page `<Head>`.

⚠️ If the HTML you send is compiled on the client, e.g. with JavaScript frameworks like React: the entity requesting your URL will need to execute the JavaScript to generate the HTML with the JSON-LD

Bonus: implement content-negotiation. If the entity requesting your URL asks for machine readable data, send them the JSON-LD describing the resource directly (instead of the HTML)

## ☑️ Properties the metadata should contains 

The metadata sent needs to contain a few required properties

1. Make sure you are properly using the URL to your resource as subject when defining the metadata
2. Provide the URL to the license with the predicate `https://schema.org/license`, `http://purl.org/dc/terms/license`, or `http://www.w3.org/1999/xhtml/vocab#license`
3. Provide the URL to download the data related to this resource using one of these predicates: `foaf:primaryTopic`, `schema:mainEntity`, `schema:distribution`, `sio:is-about`, `iao:is-about`, or `schema:codeRepository`. Use the same resource URL if you don't have a different URL to download all data related to this resource)
4. Add informations about your persistence policy with the predicate `http://www.w3.org/2000/10/swap/pim/doc#persistencePolicy`

Make sure you use predicate URIs that resolves! 

## 📋️ Example metadata for your application

We use schema.org as it is the most popular vocabulary to be well understood by search engines.

This is the metadata for your global application. It needs to be added to the header of the index page of your application.

```js
{
  "@context": [
    "http://schema.org/"
  ],
  "@id": "https://w3id.org/guide-to-fair",
  "@type": "WebApplication",
  "name": "Guide to FAIR",
  "applicationCategory": "FAIR application",
  "operatingSystem": "Any",
  "url": "https://w3id.org/guide-to-fair",
  "mainEntity": "https://w3id.org/guide-to-fair",
  "codeRepository": "https://github.com/MaastrichtU-IDS/guide-to-fair",
  "license": "https://opensource.org/licenses/MIT",
  "description": "The description of your application",
  "keywords": "FAIR data, Knowledge graph",
  "sourceOrganization": [
    {
      "@type": "Organization",
      "name": "Institute of Data Science",
      "parentOrganization": {
        "@type": "Organization",
        "name": "Maastricht University"
      }
    }
  ],
  "author": [
    {
      "@type": "Person",
      "@id": "https://orcid.org/0000-0000-0000-0000",
      "name": "Your name",
      "email": "your.email@gmail.com",
    }
}
```

Note that you will also need to generate a different JSON-LD metadata for each resource. 