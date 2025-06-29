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

![image](https://github.com/user-attachments/assets/cad9e827-94b1-4caf-a2a2-68ad544cb7f5)

### How adhesion works

In the Navu portal, you can specify a list of CSS selectors for top-adhesion, and/or a set of selectors for bottom-adhesion. 

Navu will ensure that the sidebar's top will always be below the lowest point of all the elements matching the top-adhesion selectors. Similarly, it will ensure that Sidebar is above the highest point of all the elements matching the bottom-adhesion selectors. 

You can also set CSS Variables to add some padding between the adhesion elements and the sidebar. 

```css
--nv-sidebar-adhesion-top-offset: 16;
--nv-sidebar-adhesion-bottom-offset: 16;
```

The CSS above will add 16px gap between the sidebar top/bottom. 

_**Important:** Do not add units the values of these offsets._


## CSS Styling

Typically when styling the Sidebar for a website, you would set CSS under the following three categories. 

* __Colors & Fonts__:  Colors and Fonts that affect the Sidebar and the Floating button. 
* __Docked Mode Adjustments__: When in docked mode (Classic-Docked layout), you may have to adjust the content of the page to make room for the docked sidebar
* __General Adjustments__: Any general adjustments the site independent on what mode the sidebar is in. e.g. if your site has a "_Scroll to Top_" button at the bottom-right of the page, you may want to move it to account for the sidebar. 

Having said that, Navu allows more detailed customizations as well by exposing a large set of CSS Variables. 

### Fonts

The font family can be set using the `--nv-font-family` variable. Font size and weight can be set directly on the `<navu-sidebar>` element. 

```css
navu-sidebar {
  --nv-font-family: Roboto, sans-serif;
  font-size: 14px;
  font-weight: 400;
}
```

The `font-size` sets the font of the core content - the size of the generated conversation. All other font sizes, by default will adjust based on the default font-size. e.g. the font-size of the question asked by the user is always `1.15` times the standard font-size which is used by the response. 

The goal here is to not to think of every possible font-size, you set your preferred message body font size and everything else should adapt. You could override these with CSS variables. Following are a list of font-related CSS variables you can use:

 Variable  | Description | Default  |
| ------------- | ------------- | ------------- |
| **--nv-sidebar-tab-font-size**  | Font size of the tab buttons at the top of the sidebar  | `14px` |
| **--nv-sidebar-tab-font-family**  | Override the font family for the tab buttons.  | `inherit` |
| **--nv-search-result-item-font-size**  | Font size of search response title text.  | `14px` |
| **--nv-search-result-description-font-size**  | Font size of description text of search response items.  | `13px` |
| **--nv-sidebar-citation-anchor-font-size**  | Font size of cited reference web pages in generated responses.  | `0.92em` |
| **--nv-sidebar-visitor-font-size**  | Font size of user entered question/text.  | `1.15em` |
| **--nv-sidebar-title-h2-size**  | Font size of the title text when you first load the sidebar. Typically, it says "Ask me anything"  | `2.7em` |
| **--nv-sidebar-title-h3-size**  | Font size of the secondary title text - titles on Contact page or Info page.  | `2.4em` |
| **--nv-sidebar-title-font-weight**  | Font weight of the title texts.  | `100` |
| **--nv-sidebar-suggestion-font-size**  | Font size of the suggested questions new users see.  | `1.23em` |

### Colors

The sidebar is designed primarily with four colors:

1. The background of the sidebar (`--nv-surface-rgb`)
2. The foreground color (`--nv-surface-fg-rgb`)
3. An accent color that matches your site's primary highlight color (`--nv-accent-rgb`)
4. Foreground color for content placed on the accent-color background (`--nv-accent-fg-rgb`)

We require you to set these colors as `r, g, b` values. This allows Navu to extract shades of the specified colors to highlight different items in the view. (You can overwrite these colors if you do not like the derived shades.)

So typically, the color configuration CSS looks like:

```css
:root {
  --nv-surface-rgb: 255, 240, 255;
  --nv-surface-fg-rgb: 0, 0, 0;
  --nv-accent-rgb: 46, 80, 25;
  --nv-accent-fg-rgb: 255, 255, 255;
}
```

#### Light, Dark, Colorful Themes

Navu lets you configure different color themes for the sidebar, namely **light**, **dark**, **colorful**. If you wish to support
 these themes, we recommend setting the following CSS Variables instead. 

 _**Note:** Navu onboarding team usually sets up the initial themes for you in the CSS settings. Feel free to update or replace them._

 ```cs
:root {
  --nv-light-surface-rgb: 255, 255, 255;
  --nv-light-surface-fg-rgb: 31, 31, 31;
  --nv-light-accent-rgb: 11, 77, 107;
  --nv-light-accent-fg-rgb: 255, 255, 255;

  --nv-dark-surface-rgb: 24, 24, 36;
  --nv-light-surface-fg-rgb: 255, 255, 255;
  --nv-dark-accent-rgb: 217, 222, 240;
  --nv-dark-accent-fg-rgb: 0, 0, 0;

  --nv-dark-surface-rgb: 218, 231, 203;
  --nv-light-surface-fg-rgb: 0, 0, 0;
  --nv-dark-accent-rgb: 46, 80, 25;
  --nv-dark-accent-fg-rgb: 255, 255, 255;
}
```


### Docked Mode Adjustments

You will likely need to ass some CSS if you wish to support Docked mode of the sidebar - available in the default **Classic-Dockable** layout theme. 

Docked mode ins the sidebar to the right edge of the viewport. Typically, this involves adding a padding to the right of the page to make room for the sidebar. You may decide to update the width rather than add padding. Or change some other CSS properties to ensure a docked sidebar does not overlap with your content. 

Here's some typical CSS:

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

Here `--nv-sidebar-width` is a property available to use as a value representing sidebar width. 

### General Adjustments

You may want to adjust some content on the website based on the sidebar's state independent of the layout mode of the sidebar. e.g. if your site has a "_Scroll to Top_" button at the bottom-right of the page, you may wish it to move to the left of the floating sidebar button when sidebar is closed, and hide it when sidebar is open. 

```css
.nv-sidebar-open #scrollTopButton {
  display: none;
}

.nv-sidebar-closed #scrollTopButton {
  right: calc(40px + var(--nv-minimized-sidebar-width));
}
```

Here `--nv-minimized-sidebar-width` is a property available to use as a value representing sidebar floating button width (when sidebar is closed).

### Underlying Color Themes

Based on the CSS described in the [colors section](#colors), Navu derives a lower level color theme defined by the following set of CSS variables. One could set any of these variables to override any of the auto-derived values.

| Variable  | Description | Default  |
| ------------- | ------------- | ------------- |
| **--nv-primary**  | Color of form buttons and icon buttons | `--nv-accent-rgb` |
| **--nv-on-primary** | Foreground color for above.  | `--nv-accent-fg-rgb` |
| **--nv-secondary**  | Color of highlighted messages, or when hovering of citation numbers.   | `--nv-accent-rgb` |
| **--nv-on-secondary**  | Foreground color for above.  | `--nv-accent-fg-rgb` |
| **--nv-tertiary**  | Used in feedback buttons, and any message meta properties   | `--nv-surface-fg-rgb` with opacity of `0.75` |
| **--nv-on-tertiary**  | Foreground color for above.  | `--nv-surface-rgb` |
| **--nv-highlight-surface**  | Used as background color of cited web pages in the generated answers. Also hover background for search results.  | `--nv-accent-rgb` with opacity `0.15` |
| **--nv-on-highlight-surface**  | Foreground color for above.   | `--nv-surface-fg-rgb` |
| **--nv-highlight-surface-medium**  | Used in citation references  | `--nv-accent-rgb` with opacity `0.3` |
| **--nv-on-highlight-surface-medium**  |Foreground color for above.  | `--nv-surface-fg-rgb` |
| **--nv-highlight-surface-high**  | Used in various actionable items like tabs, buttons, etc  | `--nv-accent-rgb` |
| **--nv-on-highlight-surface-high**  | Foreground color for above.  | `--nv-accent-fg-rgb` |


## Advanced CSS 

