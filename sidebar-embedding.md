# Sidebar Embedding

This document details different ways  the sidebar layout and embedding can be customized. This can be used to deeply integrate sidebar into the host website by the web developer. 

Sidebar consists of custom HTML elements `<navu-sidebar>` and `<navu-sidebar-fab>`(FAB). The FAB is the floating button that can be used to open the sidebar in its default configuration. 
`<navu-sidebar>` is the main sidebar component that can be added inside a  specific container or under the main `<body>` element. 

# Standard Embedding

When there is no extra configuration, the sidebar and FAB are added  as the last children of the `<body>` element. 
The sidebar is fixed in position to the right of the screen - spanning from top to the bottom of the window. 

See the [Layout section](./sidebar-styling.md#layout) of the styling document to see various ways to style the sidebar in different sizes. 

# Client  Side  Api & Events

Navu provides an API for the javascript on the website to control basic aspects of the sidebar. 
There are also DOM events fired by the sidebar that the code may listen to. 

The API is available at `window.$navu` global object and it implements the following interface:

```typescript
interface NavuEmbedApi {
  sidebarOpen?: boolean;
  sidebarContainer?: string | HTMLElement;
}
```

You can use this api even when Navu has not loaded yet:

```javascript
window.$navu = window.$navu || {};
window.$navu.sidebarOpen = true;
```

**sidebarOpen**: Set this property to `true` or `false` to Open/Close the default sidebar. If you are controlling the sidebar visibility yourself (See use cases below), 
you should still use this property to let the sidebar know if it's visible or hidden. This allows the sidebar to effectively lazy load the messages.

**sidebarContainer**: When you are creating a custom panel on your website to display the sidebar, you can use this to change or set that container panel. 
The value can either be a CSS selector to the element, or a reference to the element itself. 

## Events

Following events are emitted by the sidebar to support integration. More on these later. 

**sidebar-request-open**: This event is fired when the user clicks on the FAB to request the sidebar to open. 

**sidebar-request-close**: This event is fired when the user clicks on the `X` button in the sidebar to close it. 

**sidebar-error**: This event is fired when there is an error in initializing  the sidebar. The error object is available in the event's detail property `event.detail`. 
You can use this to update your UI, when  using a custom layout.

## Advanced

Following are a set of function your can call on the API after it has been initialized. 

```typescript
interface NavuEmbedApi {
  get epoch(): string;
  openSidebar(tab?: SidebarTabType, q?: string, mode?: SidebarGuideMessageMode): void;
}

type SidebarTabType = 'guide' | 'contact' | 'info';
type SidebarGuideMessageMode = 'chat' | 'search';   // Default = chat 
```

You can use `epoch` readonly property to confirm if Navu API has initialized. You can also listen to the `navu-api-ready` event on the document to get notified when API is ready.

```javascript
window.$navu = window.$navu || {};
if (window.$navu.epoch) {
  // API is ready
} else {
  document.addEventListener('navu-api-ready', () => {
     // API is ready
  );
}
```

**openSidebar**: This method can be used to Open the sidebar to a specific tab. You can optionally pass in a string that will be prefilled in the compose section. 

The `mode` attribute can be used to initialize the chat in **search** mode or **chat** mode. It defaults to **chat** mode. 

```javascript
window.$navu.openSidebar('guide', 'Tell me more about pricing');
```


# Customization Use Cases

Following are common use cases how the web developer may chose to integrate with the sidebar. 

## Custom Open/Close

Let's say you do not want a FAB on your website  but want to add an open/close button in your menu to show the sidebar. 

You can hide the FAB via CSS:

```css
navu-sidebar-fab {
  display: none;
}
```

Now you can add click handlers to your button to open/close the sidebar via the API. 

```javascript
window.$navu = window.$navu || {};
myButton.addEventListener('click', () => window.$navu.sidebarOpen = true);
```

## Custom Container with FAB

Let's say you have defined your own container where the sidebar should be rendered, could be a panel to the left of the page or perhaps a dialog. 
You control when it's opened or closed. You can still integrate with the FAB to get notified when the user wants to show the panel.

First set the custom container in the Sidebar Layout settings. Make sure to select the **'Include Floating Open Button'** option. 
<img width="644" alt="Screenshot 2025-01-02 at 6 47 28 PM" src="https://github.com/user-attachments/assets/f4831106-7a0c-4ffc-af09-e008d8a51e37" />

You can now listen to Navu events to get notified when the user wants to open/close the sidebar. Note: you should still call the API to let Navu know the open/close state of the sidebar. 

```javascript
window.$navu = window.$navu || {};

document.addEventListener('sidebar-request-open', () => {
  window.$navu.sidebarOpen = true;
  // Code to SHOW your panel
});

document.addEventListener('sidebar-request-close', () => {
  window.$navu.sidebarOpen = false;
  // Code to HIDE your panel
});
```

## Custom Container & Custom Open/Close button. 

If you are using a custom container, and decide not to use the FAB, ensure that **'Include Floating Open Button'** option is set to FALSE. 

<img width="644" alt="Screenshot 2025-01-02 at 6 53 13 PM" src="https://github.com/user-attachments/assets/28279503-c158-49cd-b258-3ac5872ecc51" />

Now you can add your own custom logic but ensure to let the API know about the state of the sidebar. 

```javascript
window.$navu = window.$navu || {};

myOpenButton.addEventListener('click', () => {
  window.$navu.sidebarOpen = true;
  // Code to SHOW your panel
});

myCloseButton.addEventListener('click', () => {
  window.$navu.sidebarOpen = false;
  // Code to HIDE your panel
});
```

## Other Customizations

### Remove Close Button

The sidebar has a `close` button. When the user clicks on it, a `sidebar-request-close` event is fired (as seen in examples above). But if you do not want the close button to show up, you can do that by setting a CSS property

```css
navu-sidebar {
--nv-drawer-close-button-display: none;
}
```

### Fullscreen mode

When not in a custom layout, the sidebar has a **Fullscreen** button. Entering fullscreen will resize the sidebar to the window size. If you do not wish to support this,  you can hide the display button via CSS.

```css
navu-sidebar {
--nv-drawer-fullscreen-button-display: none;
}
```


