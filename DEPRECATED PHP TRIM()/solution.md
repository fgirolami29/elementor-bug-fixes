
# Resolving PHP Deprecated Warning with `trim()` in Elementor Pro and Yoast SEO (PHP 8.1+)

If you're encountering the following **deprecated warnings** in your WordPress installation due to PHP 8.1+ when using Elementor Pro and Yoast SEO:

```
PHP Deprecated:  trim(): Passing null to parameter #1 ($string) of type string is deprecated in /var/www/html/wp-content/plugins/elementor-pro/modules/custom-css/module.php on line 76

PHP Deprecated:  trim(): Passing null to parameter #1 ($string) of type string is deprecated in /var/www/html/wp-content/plugins/wordpress-seo/src/presenters/canonical-presenter.php on line 49
```

These warnings occur because PHP 8.1 introduced stricter type checks, and the `trim()` function now triggers a warning when a `null` value is passed instead of a string.

### **Solution for Elementor Pro (Custom CSS Module)**

In Elementor Pro, the issue lies in the **Custom CSS module**.

1. **File Path:**
   `/wp-content/plugins/elementor-pro/modules/custom-css/module.php`

2. **Affected Line (76):**
   ```php
   return trim( $css );
   ```

3. **Fix:**
   Modify the line by casting the `$css` variable to a string to avoid passing `null` to `trim()`.

   ```php
   return \trim( (string) $css );
   ```

### **Solution for Yoast SEO (Canonical Presenter)**

In Yoast SEO, the issue is with the **Canonical URL presenter**.

1. **File Path:**
   `/wp-content/plugins/wordpress-seo/src/presenters/canonical-presenter.php`

2. **Affected Line (49):**
   ```php
   return \urldecode( \trim( \apply_filters( 'wpseo_canonical', $this->presentation->canonical, $this->presentation ) ) );
   ```

3. **Fix:**
   Modify the line to ensure the canonical URL is always cast to a string before applying `trim()`.

   ```php
   return \urldecode( (string) \trim( \apply_filters( 'wpseo_canonical', (string) $this->presentation->canonical, $this->presentation ) ) );
   ```

### **Why These Fixes Work:**

- **PHP 8.1+ Strictness**: The `trim()` function now expects a string input, and passing `null` causes a deprecated warning. The cast `(string)` ensures that even if the value is `null`, it is converted to an empty string, thus preventing the warning.
- **Fault Tolerance**: This casting approach ensures fault tolerance by making sure `trim()` always operates on a string, avoiding future issues with similar data types.

### **Steps to Implement:**

1. **Backup Your Site**: Before making any changes, always ensure you have a full backup of your WordPress files and database.
   
2. **Edit Files**: Use an FTP client or your hosting's file manager to locate and modify the following files:
   - `wp-content/plugins/elementor-pro/modules/custom-css/module.php`
   - `wp-content/plugins/wordpress-seo/src/presenters/canonical-presenter.php`

3. **Clear Cache**: After making changes, clear any caching plugins you may have installed, and refresh the pages that were throwing the error to ensure the changes are effective.

4. **Test the Site**: Visit pages using Elementor and Yoast SEO to ensure everything works correctly and the warnings are resolved.

### **Conclusion:**
These quick fixes should resolve the deprecated warnings caused by the stricter type-checking in PHP 8.1+. By ensuring that values passed to `trim()` are always strings, you can prevent these warnings and maintain compatibility with PHP 8.1+.

If you encounter further issues or if you're unsure how to proceed, it's recommended to wait for official plugin updates that fully address PHP 8.1 compatibility.


This Markdown file gives a clear step-by-step process to fix the deprecated warnings in **Elementor Pro** and **Yoast SEO**. It also provides the exact code changes, the rationale behind the solution, and how to implement the changes safely.