# Palcera Theme

A component-based Drupal theme for [Palcera CMS](https://github.com/palcera/palcera_cms), built on [Drupal Canvas](https://www.drupal.org/project/canvas) with Tailwind CSS.

## Getting started

Palcera Theme is installed automatically when you use the Palcera Base site template. It can also be installed via Composer:

```shell
composer require palcera/palcera_theme
```

### Don't sub-theme Palcera Theme

Palcera Theme does not provide backward compatibility guarantees between releases. If you need to customize beyond fonts and colors, use Drupal's starter kit tool to create your own copy:

```shell
cd drupal/web
php core/scripts/drupal generate-theme my_theme --name="My Custom Theme" --starterkit=palcera_theme
```

This creates an independent copy at `themes/my_theme` that you fully control. You can then remove the original:

```shell
composer remove palcera/palcera_theme
```

## Customizing

There are three levels of customization, from simplest to most flexible:

### 1. Fonts & colors (no build required)

Edit `theme.css` in your theme directory. This file controls CSS custom properties (design tokens) for colors, fonts, spacing, and border radius. Changes take effect after clearing Drupal's cache — no CSS rebuild needed.

Key sections in `theme.css`:

| Section | What it controls |
|---------|-----------------|
| `@theme` | Tailwind design tokens (colors, fonts, radius) |
| `:root` | CSS custom properties for light mode |
| `.dark` | CSS custom properties for dark mode |
| `@font-face` | Font files and weights |

### 2. Components & templates (build required)

Palcera Theme uses [single-directory components](https://www.drupal.org/docs/develop/theming-drupal/using-single-directory-components). Each component lives in its own folder under `components/` with a Twig template, YAML definition, and optional JS.

You can modify existing components or add new ones. Since the theme uses Tailwind, any changes to Twig templates or CSS files require a rebuild (see Building CSS below).

### 3. Full theme conversion (starter kit)

For deep customization, generate a starter kit copy (see "Don't sub-theme" above). This gives you full ownership of all templates, components, and build configuration.

## Building CSS

The theme uses [Tailwind CSS](https://tailwindcss.com) for utility-first styling. Install dependencies and build:

```shell
npm install
npm run build
```

For development with auto-rebuild on file changes:

```shell
npm run dev
```

## Code Formatting

The theme uses [Prettier](https://prettier.io) with plugins for Tailwind CSS and Twig templates.

```shell
npm run format          # Format all files
npm run format:check    # Check without changing
```

For the best experience, [set up Prettier in your editor](https://prettier.io/docs/editors) to format on save.

**Note**: Some files are excluded via `.prettierignore`, such as `html.html.twig` which contains Drupal placeholder tokens that break Prettier's parser.

## Component JavaScript

`lib/component.js` provides `ComponentType` and `ComponentInstance` classes that encapsulate Drupal behavior boilerplate:

```js
import { ComponentType, ComponentInstance } from "../../lib/component.js";

class Accordion extends ComponentInstance {
  init() {
    this.el.querySelector(".accordion--content").classList.toggle("visible");
    this.el.addClass("js");
  }
}

new ComponentType(Accordion, "accordion", ".accordion");
```

Each matching element gets its own `ComponentInstance` with `this.el` pointing to it. The `ComponentType` handles `Drupal.behaviors`, `once()`, and element discovery.

Component instances are accessible at `window.palcera_themeComponents.<name>`.

## Known issues

### Canvas code components + Tailwind

Canvas code components are not compatible with Tailwind-based themes (including Palcera Theme). Creating a code component will break the theme styling. Workaround:

1. Open Canvas's in-browser code editor, Global CSS tab.
2. Paste `theme.css` contents at the top.
3. Paste `main.css` contents below it (remove `@import` statements first).
4. Save.

This is tracked in the Canvas issue queue and applies to all Tailwind-based Canvas themes.

## Getting help

- [GitHub Issues](https://github.com/palcera/palcera_theme/issues) — bug reports and feature requests
- [Palcera CMS](https://github.com/palcera/palcera_cms) — main project
