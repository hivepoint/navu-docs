# Navu Sidebar Theming & Styling

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


