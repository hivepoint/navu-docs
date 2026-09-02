# Navu Custom CMP Integration

Navu has built in support for popular CMPs (Consent Management Platforms), like  OneTrust, TrustArc, and HubspotCMS. 

If your site uses some other CMP or if you have your own custom solution, you can integrate with Navu using a simple JavaScript API.

## Initializing the API

Add the following code in the `<head>` of your website. 

```javascript
window.$navu = window.$navu || {};
$navu.consent = 'unknown';
```

The value of **consent** can be any of these values: `'unknown' | 'granted' | 'denied'`. 

When you initialize the API, **you must set the value of consent**.  
Typically the value would be `unknown` at load time, but if the user has already granted/denied permission, the appropriate value could be set. 

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

## Adding Navu Cookies/Variables to your CMP configuration

We store information in your browser via localStorage. You can declare these in your cookies/local-storage section of your CMP configuration. 
Mark them as necessary because they are required to properly load the sidebar and respond to user requests. We do not track the user using these variables. 
Only when your CMP grants 'analytics' permission (implicitly or explicitly), we start tracking the user analytics. No new cookie is created, but an id is assigned to the `navu-embed-state` local storage variable.

`navu-embed-state` This tracks the state whether these is a unique id or not based on CMP permission, and if the sidebar was engaged by the user. 
`navu-embed-consent` This tracks the consent state, and CMP information.
`nv-sidebar` This keeps track of information regarding the sidebar (no PII) - CSS for the sidebar, What is the active tab of the sidebar, is it closed or open. This data is not sent to the server. 
