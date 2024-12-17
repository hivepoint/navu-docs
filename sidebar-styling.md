# Navu Sidebar Styling

[Theme colors](#theme-colors)

[Theme layout](#theme-layout)

[Common theming](#common-theming)

[Layout](#layout)



## Theme Colors

The sidebar comes with a default built in theme that defines a set of colors. This theme can be adjusted based on the host website by setting a bunch of CSS properties. 

*Note: For most websites you need to set only a few of these properties. More on that later.*

Following are the default properties and values that define the theme:

```css
--nv-surface-base: #ffffff;
--nv-surface-low: #fafafa;
--nv-surface-medium: #f7f7f7;

--nv-on-surface: #1D1B20;

--nv-highlight-surface: #FEF7FF;
--nv-highlight-surface-medium: #E7E0EC;
--nv-highlight-surface-high: #EADDFF;

--nv-on-highlight-surface: #1D1B20;
--nv-on-highlight-surface-medium: #49454F;
--nv-on-highlight-surface-high: #4F378B;

--nv-primary: #6750A4;
--nv-secondary: #03DAC6;
--nv-tertiary: #625B71;

--nv-on-primary: #FFFFFF;
--nv-on-secondary: #000000;
--nv-on-tertiary: #FFFFFF;

--nv-error: #B3261E;
--nv-on-error: #FFFFFF;
--nv-error-container: #F9DEDC;
--nv-on-error-container: #8C1D18;

--nv-outline:#d8d8d8;
--nv-outline-darker: #79747E;
```

The first set of these properties with the prefix `--nv-surface-` define the colors of the main surfaces - containers for content or UI controls. 
Another CSS property `-nv-on-surface` deifnes the color of the content that appears on these surfaces. Note that there is only one property for the three surfaces. 
The idea here is that these surfaces should have similar levels of brightness so the same foreground color works on all three surfaces. e.g. the default values are white, very light gray, light gray. 

The next set of properties prefixed with `--nv-highlight-` define the surfaces that are actionable (e.g. clickable) or contain special content that may need attention. There are three levels based on the importance of the content. 
Each has a corresponding property prefixed with `--nv-on-highlight-` that sets the foreground color of the content that appears on these actionable surfaces. 

The next set defines the special colors for the theme. These are used in UI controls - buttons, checkboxes, links etc. Or sometimes for getting attention like notifications or pulses. `--nv-primary` is usually the primary accent color for a site. Each of these also has a corresponding foregroung color. 

The next set defines colors used to show errors, or surfaces that contain error related content. 

Lastly, colors for borders, outlines, dividers. 

## Theme Layout 

Following is a visual map of where these properties are being used:

![Untitled-2023-10-20-2131](https://github.com/user-attachments/assets/a027fa15-215b-456d-bc19-696a588f55b2)


| Ref  | Property |
| ------------- | ------------- |
| **1**  | --nv-surface-low  |
| **2**  | --nv-on-surface  |
| **3**  | --nv-highlight-surface-high |
| **4**  | --nv-on-highlight-surface-high |
| **5**  | --nv-surface-base |
| **6**  | --nv-surface-medium |
| **7**  | --nv-tertiary |
| **8**  | --nv-primary |
| **9**  | --nv-secondary |
| **10**  | --nv-highlight-surface |
| **11**  | --nv-on-highlight-surface |
| **12** | --nv-highlight-surface-medium |

## Common Theming 

For most websites you do not need to adjust all of the properties described in the sections above. Most of the default colors are neutral enough to match most websites. 

The properties you will likely have to edit more frequently are: Main highlight color, Background color for links, Secondary color that highlights messages when they arrive, and background for tabs and suggested questions

Here are two common configuration examples:

```css
navu-sidebar {
  --nv-primary: #ec9321; // buttons, etc 
  --nv-secondary: #ec9321; // color of citations on hover, message pulsing color

  --nv-highlight-surface-high: rgb(242, 232, 221); // background of sugegstion question, tabs
  --nv-on-highlight-surface-high: rgb(242, 232, 221); // forgeround of sugegstion question, tabs

  --nv-highlight-surface: rgb(250, 247, 246); // background of citation links
}
```

```css
navu-sidebar {
  --nv-primary: rgb(0, 114, 188);
  --nv-secondary: #35bfff;
  --nv-highlight-surface-high: #0272bc;
  --nv-on-highlight-surface-high: #fff;
  --nv-highlight-surface: #eff2f4;
}
```

## Layout

For most cases, the sidebar is designed to operate in three modes based on different window wizes. One wider screens the sidebar moves the content to the left. 
On medium sized screens, the sidebar opens as an overlay on top of the content - like most chat bots. 
On smaller screens, the sidebar takes over the screen. 

![Untitled-2023-10-20-2131](https://github.com/user-attachments/assets/5985dd34-a2cd-4047-b3f7-6c7e898f6ea5)

**Note: The underlying code does not necessarily have any notion of these three modes. These need to be defined on every site via CSS. The advantage of these being CSS only is that it makes them very flexible. The disadvantage being that you have to do it on every site**

To help with the CSS, Navu will add class names to the body element indicating whether the sidebar is open or not - `nv-sidebar-open` and `nv-sidebar-closed`

Here's some sample CSS the defines these three modes:

```css
@media (min-width: 1200px) {
  body.nv-sidebar-open {
    padding-right: 400px;
  }
  .nv-sidebar-open header {
    width: calc(100% - 400px);
  }
}

@media (max-width: 1200px) and (min-width: 801px) {
  navu-sidebar::part(drawer) {
    right: 6px;
    top: 6px;
    height: auto;
    bottom: 6px;
    box-shadow: 0 5px 5px -3px rgba(0, 0, 0, .2), 0 8px 10px 1px rgba(0, 0, 0, .14), 0 3px 14px 2px rgba(0, 0, 0, .12);
    border-radius: 8px;
  }
}

@media(max-width: 800px) {
  navu-sidebar {
    --nv-drawer-width: 100%;
    --nv-sidebar-mobile: true;
  }
}
```

Let's analyze thsi CSS. 

Thew first section is under the media query `min-width: 1200px` so this defines the css for screens wider than `1200px`. In this case, we push the content from the right by `400px` which is the default width of the sidebar. Note that sometimes we add padding to the right. At other times we manipulate the width. For most cases padding should do it. If the element you're trying to resize has a width set, then you can tell it a new width by setting it to `width: calc(<old-width> - 400px);`

The second section describes the overlay mode. `(max-width: 1200px) and (min-width: 801px)` defines the styles for widths between 801 and 1200 (both inclusive). In this case we do not shift any content, but style the sidebar drawer to not go from edget to edge, and have a shadow. This gives it a visual feeling of being overalid on top of the website content. 

The last section `(max-width: 800px)` is for smaller screens. In this case we set the drawer width to be `100%` so it takes over the whole screen. **Important:** you should set the `--nv-sidebar-mobile` property to true in this case. Since this mode hides the content completely, it gives the underlying code to alter some behavior for this special case. 

## Layout Transition Points 

In the example above, it seems the transition points for the different modes is fixed. But they should be determined based on the website. 

e.g. Look at the screeenshots below

<img width="1380" alt="Screenshot 2024-12-16 at 12 43 42 PM" src="https://github.com/user-attachments/assets/9d5ea6af-ef94-43cb-8d2d-a9816de8324a" />

<img width="1314" alt="Screenshot 2024-12-16 at 12 43 52 PM" src="https://github.com/user-attachments/assets/c33127cc-201c-4931-b9aa-fbcd522be519" />

<img width="1007" alt="Screenshot 2024-12-16 at 12 47 56 PM" src="https://github.com/user-attachments/assets/1656bbdc-39ef-4763-b4a4-0ce8ef827381" />

In the first image, you will notice that sidebar works great at a certain size (1400px in this case).

In the second image,  you will notice that the sidebar is causing the menu on the side to look shrunk of badly laid out. So that is your first transition point when that happens. 

In the third image, you will see that site has replaced their menu with a mobile menu. That's a good indicator when to think about what the secont transition size should be. 

## Other styling 

You can update the font size and family as follows:

```css
nv-sidebar {
  font-size: 18px;
  --nv-font-family: monospace;
}
```

Here are some other CSS properties exposed by the sidebar:

| Property  | Description | Default  |
| ------------- | ------------- | ------------- |
| **--nv-sidebar-fab-icon-size**  | Size of the icon/image in the FAB button  | `40px` |
| **--nv-sidebar-fab-padding**  | Padding around the icon/image in the FAB  | `8px` |
| **--nv-sidebar-fab-bg**  | Override the FAB background  | `--nv-primary` |
| **--nv-sidebar-fab-fg**  | Override the FAB icon color  | `--nv-on-primary` |
| **--nv-drawer-close-button-display** | Display property of the **close** button in the sidebar. Set it to none if the  web designer wants to implement their own close button.  | -|
| **--nv-drawer-width**  | Width of the sidebar drawer  | `400px` |
| **--nv-drawer-radius**  | Border radius of the drawer  | - |
| **--nv-drawer-border-left**  | Left border of the drawer  | `1px solid var(--nv-outline)` |
| **--nv-drawer-icon-button-color**  | Color of the drawer icons - close, menu  | `--nv-tertiary` |
| **--nv-sidebar-user-icon-color**  | Color of the user/bot/agent icon in chat  | `#808080` |
| **--nv-sidebar-dots-color**  | Color of the dancing dots when message is pending  | `rgba(0, 0, 0, 0.6)` |
| **--nv-sidebar-dots-color-wave**  | Color the dancing dots animate to  | `rgba(0, 0, 0, 0.3)` |
| **--nv-sidebar-title-h2-size**  | Font size of page titles, like title of the Splash screen  | `40px` |
| **--nv-sidebar-title-h3-size**  | Font size of section titles on Info page  | `32px` |
| **--nv-sidebar-title-font-weight**  | Font weight of the section title  | `100` |
| **--nv-sidebar-title-line-height**  | Line height of the section title  | `1.2` |
| **--nv-sidebar-form-message-size**  | Font size of the text on Info page  | `18px` |
| **--nv-sidebar-form-message-weight**  | Font weight of the text on Info page  | `300` |
| **--nv-sidebar-form-input-size**  | Font size of text inputs in Sidebar forms | `16px` |
| **--nv-sidebar-citation-popup-display**  | Display property of citation popups. Set it to `none` to disable citation popups | `block` |
| **--nv-sidebar-citation-tip-bg**  | Background of citation popup | `--nv-surface-base` |
| **--nv-sidebar-citation-tip-fg**  | Foreground color of citation popup | `--nv-on-surface` |
| **--nv-sidebar-splash-bg**  | Background color of splash page | `--nv-surface-low` |
| **--nv-sidebar-form-page-bg**  | Background color of form input page | `--nv-surface-low` |




