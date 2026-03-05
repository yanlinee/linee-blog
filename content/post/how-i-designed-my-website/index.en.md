+++
title = "How I Designed an iOS-Style Personal Website"
description = "From semantic color systems to interaction motion, this post explains how I built a personal site with Apple-inspired iOS aesthetics."
summary = "A practical breakdown of applying iOS design language to a Hugo blog, including colors, typography, radius scales, glass effects, and motion design."
date = 2026-03-05T14:00:00+08:00
draft = false
categories = ["Design"]
tags = ["iOS", "Web Design", "Hugo", "CSS"]
project = "linee-blog"
entryType = "project-log"
stage = "postmortem"
+++

## Introduction

As a computer science student, I have always felt that a personal website is not just a place to showcase work, but also a reflection of design taste. While building this blog, I wanted it to feel **clean, refined, and tactile**. Apple’s iOS design language was the most natural reference for that goal.

In this post, I share how I applied iOS design principles to a Hugo blog across multiple dimensions: color system, typography, corner radius, shadow, and interaction motion.

---

## 1. Why iOS Style?

### Core Characteristics of iOS Design

| Trait                       | Explanation                                                             |
| --------------------------- | ----------------------------------------------------------------------- |
| **Clear hierarchy**         | Visual layers are built with background depth rather than heavy shadows |
| **Consistent radius scale** | A structured radius system from 6px to 24px                             |
| **Restrained color usage**  | Semantic tokens (Label, Fill, Separator) instead of hardcoded values    |
| **Refined motion**          | Natural physics feeling through spring curves                           |
| **Material realism**        | Depth created with glass-like blur and saturation                       |

These qualities make interfaces feel **professional but not cold, modern but not flashy**.

---

## 2. Color System: Semantic Colors

iOS design is powerful because it is semantic. Instead of hardcoding `#007AFF`, define `--ios-blue` and let it adapt automatically between light and dark modes.

### 2.1 Background Hierarchy

```scss
:root {
    /* Light mode */
    --ios-system-background: #ffffff;            // base page background
    --ios-secondary-system-background: #F2F2F7;  // grouped sections
    --ios-tertiary-system-background: #ffffff;   // tertiary surfaces (inputs, etc.)
}

[data-scheme="dark"] {
    /* Dark mode */
    --ios-system-background: #000000;            // OLED true black
    --ios-secondary-system-background: #1C1C1E;
    --ios-tertiary-system-background: #2C2C2E;
}
```

Using `#000000` instead of dark gray improves OLED rendering and can reduce power usage.

### 2.2 Label Hierarchy

```scss
--ios-label: #000000;                           // primary text
--ios-secondary-label: rgba(60, 60, 67, 0.6);  // secondary text
--ios-tertiary-label: rgba(60, 60, 67, 0.3);   // placeholder/disabled
```

Opacity-based layering provides better contrast consistency across different backgrounds.

### 2.3 System Colors (Auto-adaptive)

| Color | Light     | Dark      |
| ----- | --------- | --------- |
| Blue  | `#007AFF` | `#0A84FF` |
| Green | `#34C759` | `#30D158` |
| Red   | `#FF3B30` | `#FF453A` |

In dark mode, these colors are slightly brighter to maintain visual weight.

---

## 3. Typography: The Strength of SF Pro

### 3.1 Font Stack

```scss
--ios-font-family: -apple-system, BlinkMacSystemFont,
                   "SF Pro Display", "SF Pro Text",
                   "Helvetica Neue", Helvetica, Arial, sans-serif;
```

`-apple-system` allows Apple devices to use SF Pro natively. Other platforms gracefully fall back while keeping a native feel.

### 3.2 Code Font: JetBrains Mono

System fonts work for body text, but code should stay consistent with **JetBrains Mono**:

```scss
--code-font-family: "JetBrains Mono", Menlo, Monaco, Consolas, monospace;

/* Force code font */
code, pre, kbd, samp, tt {
    font-family: var(--code-font-family) !important;
}
```

JetBrains Mono’s ligatures and readability make code blocks clearer and more professional.

### 3.3 Dynamic Type Scale

| Token         | Size | Use               |
| ------------- | ---- | ----------------- |
| `large-title` | 34px | page hero title   |
| `title-1`     | 28px | post title        |
| `title-2`     | 22px | section title     |
| `headline`    | 17px | emphasized text   |
| `body`        | 17px | body content      |
| `caption`     | 12px | supporting labels |

---

## 4. Radius System: From 6px to 24px

iOS corner radius is systematic, not arbitrary:

```scss
--ios-radius-xs: 6px;       // chips, inline code blocks
--ios-radius-sm: 8px;       // buttons
--ios-radius-md: 10px;      // input controls
--ios-radius-lg: 12px;      // small widgets
--ios-radius-xl: 16px;      // standard cards
--ios-radius-2xl: 20px;     // large cards
--ios-radius-3xl: 24px;     // extra-large cards
--ios-radius-full: 9999px;  // pill controls
```

### Example Mapping

| Component      | Radius  | Effect               |
| -------------- | ------- | -------------------- |
| Top navigation | `20px`  | floating surface     |
| Article card   | `16px`  | standard card rhythm |
| Tag            | `8px`   | compact chip         |
| Button         | `999px` | pill shape           |

---

## 5. Shadows and Material: The Art of Glass

### 5.1 iOS-style Shadow

Compared with traditional heavy shadows, iOS prefers **multi-layer soft shadows**:

```scss
--shadow-ios-1: 0 1px 2px rgba(0,0,0,0.03),
                0 1px 3px rgba(0,0,0,0.05);
--shadow-ios-2: 0 3px 8px rgba(0,0,0,0.04),
                0 1px 3px rgba(0,0,0,0.06);
--shadow-ios-3: 0 6px 16px rgba(0,0,0,0.06),
                0 2px 6px rgba(0,0,0,0.04);
```

This layered approach mimics natural top lighting.

### 5.2 Glass Navigation Bar

```scss
.top-nav-inner {
    background: rgba(255, 255, 255, 0.5);
    backdrop-filter: saturate(180%) blur(20px);
    -webkit-backdrop-filter: saturate(180%) blur(20px);
    border: 0.5px solid var(--ios-separator);
}
```

Key details:
- `saturate(180%)`: boosts background color richness through blur
- `0.5px border`: signature iOS hairline precision

---

## 6. Motion System: Spring Curves

### 6.1 iOS Motion Curves

```scss
--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);        // bouncy
--ease-spring-subtle: cubic-bezier(0.25, 1, 0.5, 1);     // gentle
--ease-ios: cubic-bezier(0.4, 0, 0.2, 1);                // standard
```

### 6.2 Entrance Animation Example

```scss
@keyframes fade-up-soft {
    from {
        opacity: 0;
        transform: translateY(12px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.portal-link {
    animation: fade-up-soft 0.5s var(--ease-spring) both;
}

/* Staggered reveal */
.portal-link:nth-child(1) { animation-delay: 0ms; }
.portal-link:nth-child(2) { animation-delay: 60ms; }
.portal-link:nth-child(3) { animation-delay: 120ms; }
```

### 6.3 Interaction Feedback

```scss
.portal-link {
    transition: all 0.3s var(--ease-spring);
}

.portal-link:hover {
    transform: translateY(-3px) scale(1.01);
}

.portal-link:active {
    transform: scale(0.98);  // press feedback
}
```

---

## 7. Component Design Details

### 7.1 Timeline Component

The News page uses a vertical iOS-style timeline:

- Left rail uses `--ios-system-fill`
- Nodes use dual-layer glow (`box-shadow`)
- Dates use `tabular-nums` for stable alignment

```scss
.news-item::before {
    width: 10px;
    height: 10px;
    background: var(--accent-color);
    border-radius: 50%;
    box-shadow: 0 0 0 4px var(--card-background),
                0 0 0 5px var(--accent-color);
}
```

### 7.2 Article Cards

Cards follow an **iOS Cell** style:

- light surface + 0.5px border
- slight lift on hover (`-2px`) with richer shadow
- pill-shaped tags (`999px` radius)

### 7.3 Scrollbar

```scss
::-webkit-scrollbar {
    width: 8px;
}

::-webkit-scrollbar-thumb {
    background: var(--ios-system-fill);
    border-radius: 4px;
    border: 2px solid transparent;  // inset spacing effect
    background-clip: content-box;
}
```

The scrollbar feels visually lightweight and does not take over content space.

---

## 8. Dark Mode Adaptation

iOS dark mode is not just inversion. It is a complete design strategy:

| Element         | Light     | Dark      | Strategy                         |
| --------------- | --------- | --------- | -------------------------------- |
| Page background | `#F2F2F7` | `#000000` | true black for OLED              |
| Card background | `#FFFFFF` | `#1C1C1E` | hierarchy separation             |
| Primary text    | `#000000` | `#FFFFFF` | inversion                        |
| Secondary text  | 60% black | 60% white | preserve transparency semantics  |
| Divider         | 29% black | 60% gray  | stronger visibility in dark mode |
| Accent color    | `#007AFF` | `#0A84FF` | slight brightness boost          |

Use CSS variables and `color-mix()` to guarantee one-step theme switching.

---

## 9. Performance and Accessibility

### 9.1 Respect User Preference

```scss
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
    }
}
```

For users who prefer reduced motion, animations are effectively disabled.

### 9.2 Touch Device Optimization

```scss
@media (hover: none) {
    .card:hover {
        transform: none;  // disable hover lift on touch devices
    }

    .card:active {
        transform: scale(0.98);  // keep press feedback
    }
}
```

---

## 10. Conclusion

The key points for building an iOS-style website are:

1. **Semantic color tokens** for seamless light/dark adaptation
2. **A radius hierarchy** from 6px to 24px
3. **Soft layered shadows** with low opacity
4. **Spring-based motion** for natural interaction
5. **Glass material** using `backdrop-filter`
6. **Hairline borders** for precision details

The final result is a personal website that feels **modern, balanced, and durable**, with the refined visual quality associated with Apple design.

---

## References

- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [SF Symbols](https://developer.apple.com/sf-symbols/)
- [iOS Design System - Figma](https://www.figma.com/community/file/768365153127465917)

---

*If you are also building an iOS-inspired blog, feel free to share your setup in the comments.*
