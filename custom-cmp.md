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

We store information in your browser via localStorage. You can declare these four properties in your cookies/local-storage section of your CMP configuration. 

Three of them are to be marked as **necessary**, because they are required to properly load the sidebar and respond to user requests. We do not track the user using these variables. 

One is of them (*navu-embed-analytics*) should be marked for **analytics**  (or **tracking** based on your CMP terminology). This is set only when your CMP grants 'analytics' permission (implicitly or explicitly). When this is set, we start tracking the user analytics.

**`navu-embed-state`** (_necessary_) This tracks the state whether these is a unique id or not based on CMP permission, and if the sidebar was engaged by the user. 

**`navu-embed-consent`** (_necessary_) This tracks the consent state, and CMP information.

**`nv-sidebar`** (_necessary_) This keeps track of information regarding the sidebar (no PII) - CSS for the sidebar, What is the active tab of the sidebar, is it closed or open. This data is not sent to the server. 

**`navu-embed-analytics`** (analytics) This indicates that analytics tracking is enabled for that user. This only set when your CMP grants 'analytics' permission (implicitly or explicitly). 
