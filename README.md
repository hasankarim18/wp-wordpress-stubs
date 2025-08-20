**final, universal step-by-step setup** for **WordPress plugin development in VS Code**, optimized for **all plugins**, using **Intelephense + stubs** with a **Copilot-corrected `settings.json`**. This setup ensures autocomplete for WordPress core, Elementor (if installed), and any other plugins, while keeping VS Code fast.

---

# 📝 WordPress Plugin Dev Setup (VS Code + Intelephense + Stubs)

### composure install

## Overview

This setup gives you:

- Full **PHP + WordPress autocomplete**
- Optional **Elementor autocomplete**
- Support for common **PHP extensions** (`PDO`, `Reflection`, `cURL`, etc.)
- Minimal VS Code slowdown (no full WordPress scan)

---

## Prerequisites

1. **PHP 8.0+** installed and in PATH
2. **Composer** installed ([getcomposer.org](https://getcomposer.org))
3. **VS Code** installed
4. **Extensions in VS Code**:

   - PHP Intelephense (`bmewburn.vscode-intelephense-client`)
   - Optional: GitHub Copilot

---

## 1️⃣ Open Plugin Folder Only

- Open **only your plugin folder** in VS Code.
  Example:

  ```
  wp-content/plugins/my-awesome-plugin/
  ```

- Do **not** open the full WordPress root; this keeps indexing fast.

---

## 2️⃣ Install Stubs via Composer

Inside your plugin folder, run:

```bash
composer init -n
composer require --dev php-stubs/wordpress-stubs arifpavel/elementor-stubs
```

> ⚠️ Add `vendor/` to `.gitignore` to avoid committing stubs.

- `wordpress-stubs` → WordPress functions, hooks, classes
- `elementor-stubs` → Elementor classes (optional; still safe if Elementor not installed)

---

## 3️⃣ Configure VS Code (`.vscode/settings.json`)

Create `.vscode/settings.json` with **Copilot-corrected stub names**:

```json
{
  "intelephense.environment.includePaths": [
    "${workspaceFolder}/vendor/php-stubs/wordpress-stubs",
    "${workspaceFolder}/vendor/arifpavel/elementor-stubs"
  ],
  "intelephense.stubs": [
    "apache",
    "bcmath",
    "Core",
    "curl",
    "date",
    "json",
    "mbstring",
    "openssl",
    "PDO",
    "mysql",
    "sqlite3",
    "Phar",
    "Reflection",
    "session",
    "sockets",
    "standard",
    "superglobals",
    "tokenizer",
    "xml",
    "wordpress"
  ],
  "intelephense.diagnostics.enable": true,
  "intelephense.files.associations": ["**/*.php"],
  "intelephense.telemetry.enabled": false,
  "files.exclude": {
    "**/node_modules": true,
    "**/vendor/**/.git": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/vendor": true
  }
}
```

**Why this works:**

- Correct casing (`Core`, `PDO`, `Phar`, `Reflection`) → no red underlines in VS Code
- Includes **all major PHP extensions** for plugin dev
- Includes **WordPress core stubs** for functions and hooks
- `vendor` paths → composer stubs for WordPress + Elementor

---

## 4️⃣ Reload Intelephense Index

1. Open **Command Palette**: `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
2. Run **“Intelephense: Clear Cache and Reload”**
3. Wait a few seconds for indexing

---

## 5️⃣ Test Autocomplete

Create a test file in your plugin:

```php
<?php
add_action('init', function () {
    // WordPress autocomplete
    $url = get_option('siteurl');

    // Elementor autocomplete (if stubs installed)
    if (class_exists('\Elementor\Plugin')) {
        $widgets = \Elementor\Plugin::$instance->widgets_manager;
    }

    // PHP built-ins
    $pdo = new PDO('mysql:host=localhost;dbname=test', 'user', 'pass');
    $reflection = new ReflectionClass($pdo);
});
```

✅ You should see:

- Autocomplete for **WordPress functions/hooks**
- Elementor classes/methods (if Elementor installed)
- PHP built-in classes and extensions

---

## 6️⃣ Optional: Add More Stubs

For additional plugins/extensions:

```bash
composer require --dev php-stubs/woocommerce-stubs
composer require --dev php-stubs/acf-pro-stubs
```

Then add their paths to `"intelephense.environment.includePaths"` in `settings.json`.

---

## 7️⃣ VS Code Performance Tips

- Keep **only the plugin folder open**
- Exclude `node_modules` and `vendor` from search/index (already in settings.json)
- Disable unrelated extensions while working on PHP
- Always run **Clear Cache and Reload** after adding new stubs

---

## ✅ Result

- Fast, fully-featured PHP/WordPress/Elementor autocomplete
- No full WordPress code scan
- Proper casing avoids VS Code warnings
- Can be reused for **all plugin development projects**

---

If you want, I can also make a **ready-to-copy `composer.json`** that already includes **WordPress + Elementor stubs**, so setting up a new plugin is just **clone → composer install → VS Code reload**, fully ready.

Do you want me to make that?
