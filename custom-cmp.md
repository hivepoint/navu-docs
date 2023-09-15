# Navu Custom CMP Integration

Navu has built in support for popular CMPs (Consent Management Platforms), like  OneTrust, TrustArc, and HubspotCMS. 

If your site uses some other CMP, or have your own custom solution, you can integrate with Navu using a simple JavaScript API.

## Initializing the API

Add the following code in the `<head>` of your website. 

```javascript
window.$navu = window.$navu || {};
$navu.consent = 'unknown';
```

The value of **consent** can be any of these values: `'unknown' | 'granted' | 'denied'`. 

When you initialize the API, **you must set the value of consent**.  
Typically the value would be `unknown` at load time, but if the user has already granted/denied permission, the approopraite value could be set. 

## Updating consent

Now that the consent in the API is initialized to a default value, when the user sets or changes their consent, 
you can let Navu know by simply re-setting the property.

```javascript
$navu.consent = 'granted';
```
or 

```javascript
$navu.consent = 'denied';
```

## Navu Cookies to add to your CMP configuration

Navu uses the following cookies:

**navu-embed-browser-id**: This first-party cookie stores a unique ID assigned to a user. 

**navu-cross-domain-browser-id**: This third-party cookies is only used if your website spans across multiple domains. Typically, that is not case. 
