# Sidebar Embedding

This document details different ways  the sidebar layout and embedding can by customized. This can be used to deeply integrate sidebar into the host website by the web developer. 

Sidebar consists of to custom HTML elements `<navu-sidebar>` and `<navu-sdiebar-fab>`(FAB). The FAB is the floating button that can be used to open the sidebar in iits default configuration. 
`<navu-sidebar>` is the main sidebar component that can be added inside a  specific container or under the main `<body>` element. 

# Standard Embedding

When there is no extra configuration, the sidebar and FAB are added  as the last children of the `<body>` element. 
The sidebar is fixed in position to the right of the screen - spanning from top to the bottom of the window. 

See the [Layout section](./sidebar-styling.md#layout) of the styling document to see various ways to style the sidebar in different size. 

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

Following events are emitted by the sidebar to support integartion. More on these later. 

**sidebar-request-open**: This event is fired when the user click on the FAB to request the sidebar to open. 

**sidebar-request-close**: This event is fired when the user clicks on the `X` button in the sidebar to close it. 

**sidebar-error**: This event is fired when there as an error in initializing  the sidebar. The error object is availanle in the event's detail property. `event.detail`

# Customization Use Cases
