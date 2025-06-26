# Navu Sidebar

## Overview

**Sidebar Widget**: The sidebar is added to a page as a web component: `<navu-sidebar>`. It is automatically added as the last child of the `<body>` element. (Note:  If you want to customize how and where it is added to the page see **TODO: Custom Layout**.)

**Floating Button**: Along with the sidebar, another component is added: `<navu-sidebar-fab>`. This adds a floating button that opens the sidebar when it's closed. It has two possible modes: **Fab** and **Box** (default). Clicking on the button opens the sidebar. 

![image](https://github.com/user-attachments/assets/7dc0e1ad-95e7-4ada-82b9-44524f8850d5)

In addition to the two web components, some CSS is added to the page:

**Standard CSS**: This CSS controls the behavior of the Sidebar and defines the different layouts (discussed below **TODO: Add local link**). 

**Supplemental CSS**: This CSS can be configured in the Navu portal that is used to define the main colors and font of the sidebar. It also has CSS to conditionally affect other parts of the website when the sidebar is showing or hidden. e.g. when sidebar is docked to the right, it will add some padding to the body-right to account for the sidebar being docked there. 

**Note**: Your designers can update the Supplemental CSS in the Navu Portal, or remove it from the portal and add it as part of the website's built in CSS. 

## Body Classes

Navu will add class names to the `<body>` of the page to indicate different states or preferences set in the sidebar. These can be used to style other elements on the page. For example, to move or hide an element when the side bar is open.

```css
/* Hide the share buttons when sidebar is open */

.nv-sidebar-open #share-buttons {
  display: none;
}
```

Following are the list of class names:

**`nv-sidebar-color-${colorTheme}`**: This class is added based on which color theme is being used on the page - based on the settings in Navu portal. Values would look like `nv-sidebar-color-light`, `nv-sidebar-color-dark`, `nv-sidebar-color-custom`

**`nv-sidebar-layout-${layoutTheme}`**: This class is added to specify which layout theme is being used. e.g. `nv-sidebar-layout-classic-docked`.  (See Sidebar layouts **TODO: add link**).

**`nv-sidebar-minimized-${minimizedTheme}`**: This sets what style of floating button is being used when the sidebar is closed - fab or box. 

**`nv-sidebar-closed`**: Indicates that the sidebar is present on the page but it's in closed state. 

**`nv-sidebar-open`**: Indicates that the sidebar is open and visible. 

**`nv-sidebar-user-idle`**: Indicates that the user has not engaged with the sidebar on that pageview. 

**`nv-sidebar-user-engaged`**: Indicates that the user has engaged with the sidebar on that pageview. 


## Sidebar Layouts

In the Navu portal, you can select which Layout theme you'd like. You can choose among the following:
**Classic**, **Classic-Dockable** (default), and **Custom**. 

Selecting **Custom** means you will implement your own CSS to lay out the sidebar at different screen sizes. So following layout description will not apply. 

(*Note: You can turn off the standard CSS. If you do, then these layouts do not apply. You need to add your own CSS to make the sidebar responsive.*)

### Classic Layout

The **Classic-Dockable** (default) layout can set the sidebar in three modes based on window widths.

![image](https://github.com/user-attachments/assets/6b2e26cf-fef1-4adf-be63-0987bde3ce6c)

If you chose **Classic** layout, it's the same but without the **Docked** mode. 

![image](https://github.com/user-attachments/assets/5ebb6233-87f1-4126-a3d3-fe86398bfa9e)


**Docked**: (width: `1200px` and higher) positions the sidebar pinned to the right of the window. To allow this, the rest of the content on the page must be adjusted so the sidebar is not occluding the content. 

**Mobile**: (width: `650px` and lower) makes the sidebar open like a modal dialog on top of your content. 

**Overlay**: For the widths between Mobile and Docked mode. The sidebar appears as a floating window in the bottom  The overlay mode in itself has two different phases. When the sidebar loads in this mode, it is small in size (`~300px` in height) but when the user engages with the sidebar, it grows in size. 

![image](https://github.com/user-attachments/assets/20ebab74-54f9-4a8d-b654-8b5a155e5fb0)

#### Screen Width Transition Points

The screen widths where the modes transition can be configured in the Navu portal. The default values are `650px` to transition from Mobile to Overlay, and `1200px` to transition from Overlay to Docked. 





