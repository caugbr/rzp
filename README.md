# Require ZIP Plugin

**Require ZIP Plugin** is a utility class that ensures one or more plugins or a theme are active. If the required plugin/theme is not installed, the system provides a link to download and install it from a URL pointing to a ZIP file like a download URL in GitHub or Wordpress repository.


## Features

- **Dependency Management**: Ensures that a plugin or theme is active before allowing a dependent script to run.
- **Automatic Installation**: If the required plugin/theme is not installed, the system offers a link to download and install it from a ZIP file.
- **Admin Notices**: Displays a warning in the WordPress admin area until the required plugin/theme is activated.
- **Flexible**: Supports both plugins and themes.


## Usage

### Requiring a Plugin or Theme

To add a plugin or theme to the list of required items, instantiate the `RequireZipPlugin` class and use the `require` method. The `require` method accepts the following parameters:

- **`$dependent`** (string): The name of the script that requires the plugin/theme.
- **`$required`** (string): The name of the required plugin/theme.
- **`$zip_url`** (string): The URL pointing to the ZIP file for installation.
- **`$plugin_id`** (string): The ID of the required plugin/theme as used internally by WordPress (e.g., `folder_name/file_name.php` for plugins or `theme-folder/style.css` for themes).
- **`$type`** (string, optional): The type of the required script (`'plugin'` or `'theme'`). Default: `'plugin'`.


### Example

Suppose the plugin **Inline Edit** depends on functionalities provided by **WP Helper**, which is hosted on GitHub. **Inline Edit** will not initialize unless **WP Helper** is active. Until **WP Helper** is activated, a warning will be displayed on all admin pages.

```php
require_once plugin_dir_path(__FILE__) . "/rzp/require-zip-plugin.php";

class InlineEdit {
    public function __construct() {
        global $wp_helper;

        // Require a plugin
        $require_zip_plugin = new RequireZipPlugin();
        $require_zip_plugin->require(
            'Inline Edit', // Dependent script
            'WP Helper',   // Required plugin
            'https://github.com/caugbr/wp-helper/archive/refs/heads/main.zip', // ZIP URL
            'wp-helper/wp-helper.php' // Plugin ID
        );

        // Require a theme
        $require_zip_plugin->require(
            'Inline Edit', // Dependent script
            'Vue WP Theme', // Required theme
            'https://github.com/caugbr/vue-wp-theme/archive/refs/heads/main.zip', // ZIP URL
            'vue-wp-theme/style.css', // Theme ID (path to style.css)
            'theme' // Specify that it's a theme
        );

        // Check if the required plugin is active
        if ($wp_helper) {
            // Your plugin initialization code here
        }

        // Check if the required theme is active
        if (wp_get_theme()->get('Name') == 'Vue WP Theme') {
            // Your theme-specific code here
        }
    }
}
```


### Displayed Messages

When a required plugin or theme is missing or inactive, the system displays a message in the WordPress admin area. The message includes:

1. A notice that the current script (`$dependent`) depends on the required plugin/theme (`$required`).
2. A link to download and install the required plugin/theme from the provided ZIP URL.
3. A prompt to activate the plugin/theme after installation.

You can customize these messages by translating the `.po` file located in the `langs` directory. The placeholders (`%1$s`, `%2$s`, etc.) correspond to the following:

- `%1$s`: The name of the dependent script.
- `%2$s`: The name of the required plugin/theme.
- `%3$s`: The opening tag for the link (e.g.,  `<a href="...">`).
- `%4$s`: The closing tag for the link (e.g.,  `</a>`).


## Translation

To translate the messages, edit the `.po` file located in the `langs` directory. For example, the file `rzp-pt_BR.po` contains the Portuguese (Brazil) translations. After editing the `.po` file, compile it into a `.mo` file using a tool like [Poedit](https://poedit.net/).


## Directory Structure

The `Require ZIP Plugin` class is accompanied by a `langs` directory containing translation files. When using this class in your plugins or themes, place the `rzp` directory in the root of your project. For example:
```
/your-plugin/
├── rzp/
│ ├── require-zip-plugin.php
│ └── langs/
│ ├── rzp-pt_BR.mo
│ ├── rzp-pt_BR.po
│ └── rzp.pot
└── your-plugin.php
```
