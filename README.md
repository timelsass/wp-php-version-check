# wp-php-version-check
Composer package to verify that WordPress and PHP versions satisfy a minimum version requirement.

-------------------------------------------

Using composer, you can get started quickly:

```php
$ composer require timelsass/wp-php-version-check
```

We aren't going to be using the autoloader for this package because WordPress supports PHP 5.2 as a minimum, and this is just a single class.  In your code you would add to your main plugin file after the headers, something like this:

```php
if ( ! class_exists( 'Wp_Php_Version_Check' ) ) {
  require plugin_dir_path( __FILE__ ) . 'vendor/timelsass/wp-php-version-check/class-wp-php-version-check.php';
}
```

The init method takes three parameters:

```php
Wp_Php_Version_Check::init( $plugin, $wp_version, $php_version );
```

1. `$plugin` - The main plugin file relative to the plugins directory.  Usually you would use: `plugin_basename( __FILE__ )`.
2. `$wp_version` - (default is 4.7) The minimum WordPress version required for your plugin to work.
3. `$php_version` - (default is 5.3) The minimum PHP version required for your plugin to work.

You simply need to initialize the version checking with the necessary parameters, like this:

```php
Wp_Php_Version_Check::init( plugin_basename( __FILE__ ), '4.7', '5.3' );
```

This will do the version checking portion of things for you, so your next step would be to add a hook telling WordPress what to do to initialize your plugin's actual code.

The hook name is dynamically generated based on $plugin.  If you have a plugin called "example-plugin", then the hook generated would be "example-plugin:init".

This code shows how to initalize some code for a plugin called "example-plugin" in an anonymous function (anonymous function passed on a hook requires PHP 5.3+):

```php
add_action( 'example-plugin:init', function() {
  wp_die( 'Passes Version Requirements.' );
});
```

Here's a full example that you can use:

```php
/**
 * Plugin Name: Example Plugin
 * Plugin URI: https://github.com/timelsass/wp-php-version-check
 * Description: This is an example of using Wp_Php_Version_Check class.
 * Version: 1.0.0
 * License: GPLv2 or later
 */
 
// Prevent direct calls.
defined( 'WPINC' ) ? : die;

// Load the Wp_Php_Version_Check file and make sure that the class doesn't exist first.
if ( ! class_exists( 'Wp_Php_Version_Check' ) ) {
  require plugin_dir_path( __FILE__ ) . 'vendor/timelsass/wp-php-version-check/class-wp-php-version-check.php';
}

// Initalize the version checking.  This checks that the user has at least WordPress v4.0 and PHP v5.6.
Wp_Php_Version_Check::init( plugin_basename( __FILE__ ), '4.0', '5.6' );

// Initalize our core plugin functionality in the example-plugin:init hook.
add_action( 'example-plugin:init', 'example_plugin_load' );

/**
 * Kicks off our core plugin code.
 */
function example_plugin_load() {
  wp_die( 'Example plugin passes the version check requirements! Woohoo!' );
}
```
