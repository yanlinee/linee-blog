+++
title = "如何设计一个 iOS 风格的个人主页"
description = "从色彩系统到交互动画，分享我如何打造一个具有苹果 iOS 美学的设计"
date = 2026-03-05T14:00:00+08:00
draft = false
categories = ["设计"]
tags = ["iOS", "Web Design", "Hugo", "CSS"]
project = "linee-blog"
entryType = "project-log"
stage = "postmortem"
+++

## 前言

作为一名计算机专业的学生，我一直觉得个人主页不仅是展示作品的窗口，更是设计品味的体现。在打造这个博客的过程中，我希望它能呈现出一种**简洁、精致、有质感**的视觉风格——而苹果的 iOS 设计语言无疑是最符合这一理念的参考。

这篇文章将分享我如何将 iOS 的设计哲学应用到 Hugo 博客中，涵盖色彩系统、排版、圆角、阴影、动画等多个维度。

---

## 一、为什么选择 iOS 风格？

### iOS 设计的核心特质

| 特质         | 说明                                               |
| ------------ | -------------------------------------------------- |
| **层级清晰** | 通过背景色深浅建立视觉层级，而非单纯依赖阴影       |
| **圆角统一** | 从 6px 到 24px 的层级化圆角系统                    |
| **色彩克制** | 使用语义化颜色（Label、Fill、Separator）而非硬编码 |
| **动效细腻** | 弹簧曲线（Spring）带来的自然物理感                 |
| **材质真实** | 毛玻璃效果（Blur + Saturate）营造深度              |

这些特质让界面看起来**专业而不冰冷，现代而不浮夸**。

---

## 二、色彩系统：Semantic Colors

iOS 最大的智慧在于**语义化颜色**。与其写死 `#007AFF`，不如定义 `--ios-blue`，并在深浅模式下自动切换。

### 2.1 背景色层级

```scss
:root {
    /* 浅色模式 */
    --ios-system-background: #ffffff;           // 最底层（页面背景）
    --ios-secondary-system-background: #F2F2F7; // 次级背景（分组）
    --ios-tertiary-system-background: #ffffff;  // 第三层（输入框等）
}

[data-scheme="dark"] {
    /* 深色模式 */
    --ios-system-background: #000000;           // OLED 纯黑
    --ios-secondary-system-background: #1C1C1E;
    --ios-tertiary-system-background: #2C2C2E;
}
```

使用 `#000000` 而非深灰色是为了**优化 OLED 屏幕显示**，同时减少耗电。

### 2.2 标签色层级

```scss
--ios-label: #000000;                        // 主文字
--ios-secondary-label: rgba(60, 60, 67, 0.6); // 次要文字
--ios-tertiary-label: rgba(60, 60, 67, 0.3);  // 禁用/占位
```

使用透明度而非固定灰色，确保在任意背景下都有良好的对比度。

### 2.3 系统色（自动适配深浅模式）

| 颜色  | 浅色模式  | 深色模式  |
| ----- | --------- | --------- |
| Blue  | `#007AFF` | `#0A84FF` |
| Green | `#34C759` | `#30D158` |
| Red   | `#FF3B30` | `#FF453A` |

深色模式的色彩会**稍微提亮**，保持视觉重量一致。

---

## 三、字体系统：SF Pro 的精髓

### 3.1 字体栈

```scss
--ios-font-family: -apple-system, BlinkMacSystemFont, 
                   "SF Pro Display", "SF Pro Text", 
                   "Helvetica Neue", Helvetica, Arial, sans-serif;
```

使用 `-apple-system` 让 macOS/iOS 设备自动调用** SF Pro **，Windows 设备回退到 Segoe UI，保持原生感。

### 3.2 代码字体：JetBrains Mono

虽然正文使用系统字体，但代码必须统一使用 **JetBrains Mono**：

```scss
--code-font-family: "JetBrains Mono", Menlo, Monaco, Consolas, monospace;

/* 强制应用 */
code, pre, kbd, samp, tt {
    font-family: var(--code-font-family) !important;
}
```

JetBrains Mono 的**连字特性（Ligatures）**和**可读性**让它成为代码展示的最佳选择。

### 3.3 Dynamic Type 层级

| Token         | 尺寸 | 用途       |
| ------------- | ---- | ---------- |
| `large-title` | 34px | 页面大标题 |
| `title-1`     | 28px | 文章标题   |
| `title-2`     | 22px | 章节标题   |
| `headline`    | 17px | 强调正文   |
| `body`        | 17px | 普通正文   |
| `caption`     | 12px | 辅助说明   |

---

## 四、圆角系统：从 6px 到 24px

iOS 的圆角不是随意的，而是有严格的层级：

```scss
--ios-radius-xs: 6px;       // 小标签、代码块
--ios-radius-sm: 8px;       // 按钮
--ios-radius-md: 10px;      // 输入框
--ios-radius-lg: 12px;      // 小组件
--ios-radius-xl: 16px;      // 标准卡片
--ios-radius-2xl: 20px;     // 大卡片
--ios-radius-3xl: 24px;     // 特大卡片
--ios-radius-full: 9999px;  // 胶囊按钮
```

### 应用示例

| 组件     | 圆角值  | 效果     |
| -------- | ------- | -------- |
| 顶部导航 | `20px`  | 悬浮感   |
| 文章卡片 | `16px`  | 标准卡片 |
| 标签     | `8px`   | 紧凑     |
| 按钮     | `999px` | 胶囊形   |

---

## 五、阴影与材质：毛玻璃的艺术

### 5.1 iOS 风格阴影

相比传统的重阴影，iOS 偏好**多层柔和阴影**：

```scss
--shadow-ios-1: 0 1px 2px rgba(0,0,0,0.03), 
                0 1px 3px rgba(0,0,0,0.05);
--shadow-ios-2: 0 3px 8px rgba(0,0,0,0.04), 
                0 1px 3px rgba(0,0,0,0.06);
--shadow-ios-3: 0 6px 16px rgba(0,0,0,0.06), 
                0 2px 6px rgba(0,0,0,0.04);
```

这种"分层阴影"模拟了**光源从上方照射**的自然效果。

### 5.2 毛玻璃导航栏

```scss
.top-nav-inner {
    background: rgba(255, 255, 255, 0.5);
    backdrop-filter: saturate(180%) blur(20px);
    -webkit-backdrop-filter: saturate(180%) blur(20px);
    border: 0.5px solid var(--ios-separator);
}
```

关键点：
- `saturate(180%)`：增强背景色彩的饱和度，让模糊区域更有"活力"
- `0.5px border`：iOS 特有的**半像素分隔线**，极致精致

---

## 六、动画系统：Spring 曲线

### 6.1 iOS 动画曲线

```scss
--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);        // 弹跳感
--ease-spring-subtle: cubic-bezier(0.25, 1, 0.5, 1);     // 柔和
--ease-ios: cubic-bezier(0.4, 0, 0.2, 1);                // 标准
```

### 6.2 入场动画示例

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

/* 交错动画 */
.portal-link:nth-child(1) { animation-delay: 0ms; }
.portal-link:nth-child(2) { animation-delay: 60ms; }
.portal-link:nth-child(3) { animation-delay: 120ms; }
```

### 6.3 交互反馈

```scss
.portal-link {
    transition: all 0.3s var(--ease-spring);
}

.portal-link:hover {
    transform: translateY(-3px) scale(1.01);
}

.portal-link:active {
    transform: scale(0.98);  // 按压感
}
```

---

## 七、组件设计细节

### 7.1 时间线组件

News 页面的时间线采用了 iOS 风格的**纵向时间轴**：

- 左侧细线使用 `--ios-system-fill` 背景
- 节点使用带光晕的圆点（`box-shadow` 双层）
- 日期使用 `tabular-nums` 保持数字等宽

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

### 7.2 文章卡片

采用 **iOS Cell 风格**：

- 白色背景 + 0.5px 边框
- 悬停时轻微上浮（`-2px`）+ 阴影增强
- 标签使用胶囊形状（`999px` 圆角）

### 7.3 滚动条

```scss
::-webkit-scrollbar {
    width: 8px;
}

::-webkit-scrollbar-thumb {
    background: var(--ios-system-fill);
    border-radius: 4px;
    border: 2px solid transparent;  // 边距效果
    background-clip: content-box;
}
```

滚动条**悬浮在内容边缘**，而非占据空间。

---

## 八、深色模式适配

iOS 的深色模式不是简单的反色，而是一套完整的设计系统：

| 元素     | 浅色      | 深色      | 策略         |
| -------- | --------- | --------- | ------------ |
| 页面背景 | `#F2F2F7` | `#000000` | 纯黑省电     |
| 卡片背景 | `#FFFFFF` | `#1C1C1E` | 层级区分     |
| 主文字   | `#000000` | `#FFFFFF` | 反转         |
| 次级文字 | 60% 黑    | 60% 白    | 保持透明度   |
| 分隔线   | 29% 黑    | 60% 灰    | 深色下更可见 |
| 强调色   | `#007AFF` | `#0A84FF` | 提亮         |

使用 `color-mix()` 或 CSS 变量确保**一键切换**。

---

## 九、性能与可访问性

### 9.1 尊重用户偏好

```scss
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
    }
}
```

对于**减少动画**偏好的用户，完全禁用动效。

### 9.2 触摸设备优化

```scss
@media (hover: none) {
    .card:hover {
        transform: none;  // 禁用悬停效果
    }
    
    .card:active {
        transform: scale(0.98);  // 保留按压反馈
    }
}
```

---

## 十、总结

打造 iOS 风格的关键在于：

1. **使用语义化颜色** —— 深浅模式无缝切换
2. **层级化的圆角** —— 从 6px 到 24px 的系统
3. **柔和的阴影** —— 多层低透明度阴影
4. **弹簧动画** —— 自然的物理曲线
5. **毛玻璃材质** —— `backdrop-filter` 营造深度
6. **半像素边框** —— 极致的细节追求

最终效果是一个**既现代又耐看**的个人主页，在任何设备上都能呈现出苹果设计特有的精致感。

---

## 参考资源

- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [SF Symbols](https://developer.apple.com/sf-symbols/)
- [iOS Design System - Figma](https://www.figma.com/community/file/768365153127465917)

---

*如果你也想为自己的博客添加 iOS 风格，欢迎在评论区交流！*
