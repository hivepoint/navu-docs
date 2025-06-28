# Navu Sidebar

When the Sidebar is added to a web page, two components get appended to the `<body>` of the page. 

**Floating Button**: A web component `<navu-sidebar-fab>` that adds a floating action button (FAB) or a 'Ask me anything' textbox (Box) at the bottom-right of the page. Clicking on these opens the Sidebar. When Sidebar is open, the floating button is not visible. Settings in the Navu portal determine whether to shoa a **FAB** or a **Box**. 

![image](https://github.com/user-attachments/assets/7dc0e1ad-95e7-4ada-82b9-44524f8850d5)

**Sidebar Widget**: The sidebar is added to a page as a web component: `<navu-sidebar>`. (Note:  If you want to customize how and where it is added to the page see **TODO: Custom Layout**.)


## Sidebar Widget Overview

In the Navu Portal you can set the *layout theme* of the sidebar. The current version of Navu allows three possible values for this layout: **Classic**, **Classic-Dockable**, and **Custom**. If nothing is selected, **Classic-Dockable** is used, as this is the most popular and most responsive built-in layout. 

Based on the *layout theme* selection, Navu will inject CSS in the `<head>` of your page. This CSS lays out the sidebar in different screen widths. 

If you selected **Custom**, then no CSS is injected. And the Website developers are responsible of customizing the layout. By default the Sidebar will appear `position: fixed` docked to the right of the screen. 

### Additional CSS

You can add additional CSS to style the Sidebar based on your website, e.g. to customize the colors of font of the sidebar. You may also need CSS to change your site layout to allow for the sidebar to dock nicely with the rest of the content. 

YOu can specify the custom CSS in the Navu portal - Navu will inject this CSS when the Sidebar is created; or you can add the CSS in your Website's code. 

_**Note:** Navu's onboarding team typically adds some CSS in the Navu Portal so that the sidebar loads nicely in your website. This way the sidebar is up and running on the website without and developer/designer involvement. Feel free to update it if needed._

### Body Classes 

Navu will add class names to the `<body>` of the page to indicate different states or preferences set in the sidebar. These can be used to style other elements on the page. For example, to move or hide an element when the sidebar is open.

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

In the Navu portal, you can select which layout theme you'd like. You can choose among the following:
**Classic**, **Classic-Dockable** (default), and **Custom**. 

_Selecting **Custom** means you will implement your own CSS to lay out the sidebar at different screen sizes. So following layout description will not apply._

### Classic / Classic-Dockable Layout

The **Classic-Dockable** (default) layout can set the sidebar in three modes based on window widths.

![image](https://github.com/user-attachments/assets/6b2e26cf-fef1-4adf-be63-0987bde3ce6c)

If you chose **Classic** layout, it's the same but without the **Docked** mode. 

![image](https://github.com/user-attachments/assets/5ebb6233-87f1-4126-a3d3-fe86398bfa9e)


**Docked**: (width: `1200px` and higher) positions the sidebar pinned to the right of the window. To allow this, the rest of the content on the page must be adjusted so the sidebar is not occluding the content. 

**Mobile**: (width: `650px` and lower) makes the sidebar open like a modal dialog on top of your content. 

**Overlay**: For the widths between Mobile and Docked mode. The sidebar appears as a floating window in the bottom  The overlay mode in itself has two different phases. When the sidebar loads in this mode, it is small in size (`~300px` in height) but when the user engages with the sidebar, it grows in size. 

![image](https://github.com/user-attachments/assets/20ebab74-54f9-4a8d-b654-8b5a155e5fb0)

### Screen Width Transition Points

The screen widths where the modes transition can be configured in the Navu portal. The default values are `650px` to transition from Mobile to Overlay, and `1200px` to transition from Overlay to Docked. 

#### Using media queries in CSS

These transition points can be used in CSS media queries to update properties for specific modes. For example, following CSS changes the z-index of the navu-sidebar only in docked mode. Notice that the selector includes the class name for the *classic-dockable* theme so it only applies when that theme is selected.

```css
@media (min-width: 1200px) {
  .nv-sidebar-layout-classic-dockable navu-sidebar {
    z-index: 1000;
  }
}
```

**If you are setting the CSS in Navu Portal**, you can use _**special fields**_ instead of actual `px` values for these transition points. This is preferred because if the transition widths are changes in the settings, then the CSS will automatically be updated. 

Following are these special properties:

**`%DockedToOverlayWidth%`**: For **Classic-Docked** layout - Width (without units) at which Docked mode turns into Overlay mode. *Note:* When the width is exactly the set value, Docked mode is used. Any value below that will set the sidebar to the Overlay mode. To style the Overlay mode in a media query use `%DockedToOverlayWidthMinus%`.

**`%OverlayToMobileWidth%`**: Width below which (inclusive), the Mobile mode will be used. When width is higher than this value, Overlay mode kicks in. 

**`%DockedToOverlayWidthMinus%`**: For **Classic-Docked** layout - Max width (inclusive) when the Overlay mode is used. 

**`%OverlayToMobileWidthPlus%`**: Min width (inclusive) when the Overlay mode is used. 

When using these special properties, Here's the CSS in Classic-Dockable layout to set the z-index to different values in different modes:

```css
/* Set the z-index to 1000 when in docked mode */
@media (min-width: %DockedToOverlayWidth%px) {
  .nv-sidebar-layout-classic-dockable navu-sidebar {
    z-index: 1000;
  }
}

/* Set the z-index to 9000 when in mobile mode */
@media (max-width: %OverlayToMobileWidth%px) {
  .nv-sidebar-layout-classic-dockable navu-sidebar {
    z-index: 9000;
  }
}

/* Set the z-index to 5000 when in overlay mode */
@media (max-width: %DockedToOverlayWidthMinus%px) and (min-width: %OverlayToMobileWidthPlus%px) {
  .nv-sidebar-layout-classic-dockable navu-sidebar {
    z-index: 5000;
  }
}
```

## Adhesion Based Layout

Independent of the Layout-theme, Navu provides adhesion based constraints on the top and bottom position of the sidebar. What that means is you can specify that the Sidebar will always remain below a specific element on the page, or above a specified element. 

Most common scenario is when you have a dynamic-sized header on the page. The header may change size or scroll on/off the viewport as the user scrolls. Navu will ensure that the sidebar's top will always be below that header. 

**TODO: write this**

## CSS Styling

Typically when styling the Sidebar for a website, you would set CSS under the following three categories. See Advanced CSS Styling (**TODO: link here**) to see more styling opportunities. 

* __Colors & Fonts__:  Colors and Fonts that affect the Sidebar and the Floating button. 
* __Docked Mode Adjustments__: When in docked mode, you may have to adjust the content of the page to make room for the docked sidebar
* __General Adjustments__: Any general adjustments the site independent on what mode the sidebar is in. e.g. if your site has a "_Scroll to Top_" button at the bottom-right of the page, you may want to move it to account for the sidebar. 

### Colors & Font

The Standard CSS included with the widget exposes some CSS properties to easily configure visual theme of the sidebar. 

The font family is set using `--nv-font-family` property. 

Typically you want to set the following colors:

**Background & Foreground Color**: Background color of the sidebar and color of the text that appears in the foreground. 

**Accent Color**: An accent color to highlight actionable or special items in the sidebar, like buttons, citations, etc. And a foreground accent color - Color of text or icons which appear on top of an accented surface. 

Typically you would want to se these colors for both **Light** and **Dark** themes. All these colors have reasonable default values set, if you do not specify any one of them.

_**Note**: These theme based Colors are specified as `r, g, b` values rather than standard CSS colors. These `r, g, b` values are used by the standard template to derive other shades of this color._

```cs
:root {
  --nv-font-family: Roboto, sans-serif;

  --nv-light-accent-rgb: 11, 77, 107;
  --nv-light-accent-fg-rgb: 255, 255, 255;

  --nv-dark-accent-rgb: 217, 222, 240;
  --nv-dark-accent-fg-rgb: 0, 0, 0;

  --nv-light-surface-rgb: 255, 255, 255;
  --nv-light-surface-fg-rgb: 31, 31, 31;

  --nv-dark-surface-rgb: 24, 24, 36;
  --nv-light-surface-fg-rgb: 255, 255, 255;
}
```

See a complete list of CSS properties available here (**TODO: add link here**)

### Docked Mode Adjustments

You will likely need some CSS if you wish to support Docked mode (default) of the sidebar. 

Typically this involves adding a padding to the right of the page to make room for the sidebar. You may decide to update the width rather than add padding. Or change some other CSS properties to ensure a docked sidebar does not overlap with your content. 

Here's some typical CSS

```css
/* Following CSS only applies when sidebar is Docked */

@media (min-width: %DockedToOverlayWidth%px) {
  /* Add right-padding to the body */
  body.nv-sidebar-layout-classic-dockable.nv-sidebar-open {
    padding-right: var(--nv-sidebar-width);
  }

  /* Add the a top border to the sidebar when it is docked */
  .nv-sidebar-layout-classic-dockable navu-sidebar {
    --nv-drawer-border-top: 1px solid;
  }

  /* Adjust the width of the carousel */
  .nv-sidebar-layout-classic-dockable .my-carousel {
    width: calc(100% - var(--nv-sidebar-width));
  }
}
```

Here `--nv-sidebar-width` is a property provided by the standard CSS that you can use to use as a variable representing sidebar width. Read more about this here (**TODO: Add link**)

### General Adjustments

You may want to adjust some content on the website based on the sidebar's state independent of the layout mode of the sidebar. e.g. if your site has a "_Scroll to Top_" button at the bottom-right of the page, you may want it to move to the left of the floating sidebar button when sidebar is closed, and hide it when sidebar is open. 

```css
.nv-sidebar-open #scrollTopButton {
  display: none;
}

.nv-sidebar-closed #scrollTopButton {
  right: calc(40px + var(--nv-minimized-sidebar-width));
}
```
Here `--nv-minimized-sidebar-width` is a property provided by the standard CSS that you can use to use as a variable representing sidebar floating button width when sidebar is closed. Read more about this here (**TODO: Add link**)

## Advanced CSS 

Navu Sidebar exposes a lot of CSS variables that can be used to fine tune styling of the sidebar based on the website's needs. 

**The standard CSS**, typically included with the sidebar abstracts out some of these CSS properties to some common ones. e.g. if you set the accent color, it will set the various variables based on the accent color - creating different shaded based on the accent color. The standard CSS also adjusts the layout to support different modes discussed above. 

If you disable Standard CSS (in the Navu portal), you are responsible for laying out and overriding all the colors. 

Following are the list of CSS variables available to you. Some of them are available via the standard CSS. Others are available to all even if you have the standard CSS disabled. 

### Standard CSS Variables

| Variable  | Description | Default  |
| ------------- | ------------- | ------------- |
| **--nv-sidebar-docked-width**  | Width of the sidebar when in Docked mode.  | `320px` |
| **--nv-sidebar-overlay-width**  | Width of the sidebar when in Overlay mode.  | `420px` |
| **--nv-overlay-drawer-shadow**  | Drop shadow of the sidebar when in Overlay mode.  | `--nv-primary` |
| **--nv-sidebar-docked-z-index**  | Z-index of sidebar in docked mode  | `100` |
| **--nv-sidebar-overlay-z-index**  | Z-index of sidebar in overlay mode  | `100` |
| **--nv-sidebar-mobile-z-index**  | z-index of the modal sidebar when in mobile mode  | `2147483647` |
| **--nv-sidebar-overlay-max-height**  | Max-height of the sidebar when in Overlay mode  | `900px` |
| **--nv-sidebar-overlay-max-idle-height**  | Max-height of the sidebar when in Overlay mode, before the use has engaged with the sidebar  | `300px` |
| **--nv-light-accent-rgb**  | Accent color RGB value in light mode  | `11, 77, 107` |
| **--nv-light-accent-fg-rgb**  | Foreground color for text and icons that appear on top of accent color in light mode  | `255, 255, 255` |
| **--nv-light-surface-rgb**  | Surface background color of the sidebar in light mode  | `255, 255, 255` |
| **--nv-light-surface-fg-rgb**  | Foreground color for text and icons that appear on top of sidebar in light mode  | `0, 0, 0` |
| **--nv-dark-accent-rgb**  | Accent color RGB value in dark mode  | `217, 222, 240` |
| **--nv-dark-accent-fg-rgb**  | Foreground color for text and icons that appear on top of accent color in dark mode  | `0, 0, 0` |
| **--nv-dark-surface-rgb**  | Surface background color of the sidebar in dark mode  | `0, 0, 0` |
| **--nv-dark-surface-fg-rgb**  | Foreground color for text and icons that appear on top of sidebar in dark mode  | `255, 255, 255` |
| **--nv-colorful-accent-rgb**  | Accent color RGB value in colorful mode  | `46, 80, 25` |
| **--nv-colorful-accent-fg-rgb**  | Foreground color for text and icons that appear on top of accent color in colorful mode  | `255, 255, 255` |
| **--nv-colorful-surface-rgb**  | Surface background color of the sidebar in colorful mode  | `218, 231, 203` |
| **--nv-colorful-surface-fg-rgb**  | Foreground color for text and icons that appear on top of sidebar in colorful mode  | `0, 0, 0` |

## Core CSS Variables

These are the set of variables defined and used by the navu sidebar widget. Even if you are not using the Standard CSS - i.e. doing your own layout, you can set these properties on the `<navu-sidebar>` to style and configure the component.

| Variable  | Description | Default  |
| ------------- | ------------- | ------------- |
