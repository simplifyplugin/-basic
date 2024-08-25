# WordPress & PHP Concepts

## 1. **Session and Cookies**

- **Session**: 
  - Stores data on the server for a single user and is available to all pages in one application.
  - Typically used to keep track of user data across pages (e.g., login status).
  - Data is stored on the server-side and identified by a session ID stored in a cookie on the client-side.
  
- **Cookies**: 
  - Stores data on the client-side (browser) and is sent back to the server with each request.
  - Used to remember information like user preferences, login sessions, etc.
  - Persistent across sessions until they expire or are deleted.

| **Aspect**   | **Session**                           | **Cookies**                              |
|--------------|---------------------------------------|------------------------------------------|
| **Storage**  | Server-side                           | Client-side (browser)                    |
| **Duration** | Temporary (typically ends with logout or session timeout) | Persistent (until expiration date) |
| **Security** | More secure, as data is stored server-side | Less secure, as data is stored client-side |

## 2. **Transients**

- **Transients** are a way of storing cached data in the WordPress database with an expiration time.
- They are used to store temporary data, reducing the number of database queries and improving performance.
- Example:
  ```php
  set_transient( 'my_transient_key', 'my_value', 12 * HOUR_IN_SECONDS );
  $value = get_transient( 'my_transient_key' );
  ```

## 3. **How to Enqueue Script and Style?**

- In WordPress, the proper way to include scripts and styles is by using the `wp_enqueue_script()` and `wp_enqueue_style()` functions.
- Example:
  ```php
  function my_theme_enqueue_styles() {
      wp_enqueue_style( 'style-name', get_stylesheet_uri() );
  }
  add_action( 'wp_enqueue_scripts', 'my_theme_enqueue_styles' );

  function my_theme_enqueue_scripts() {
      wp_enqueue_script( 'script-name', get_template_directory_uri() . '/js/script.js', array(), '1.0.0', true );
  }
  add_action( 'wp_enqueue_scripts', 'my_theme_enqueue_scripts' );
  ```

## 4. **How to Optimize a Website?**

- **Caching**: Implement caching (e.g., using plugins like WP Super Cache).
- **Image Optimization**: Compress images using tools like Smush or TinyPNG.
- **Minification**: Minify CSS, JavaScript, and HTML files.
- **Database Optimization**: Regularly clean and optimize the database.
- **CDN**: Use a Content Delivery Network (CDN) like Cloudflare.
- **Lazy Loading**: Implement lazy loading for images and videos.

## 5. **How to Create a Child Theme?**

- Create a new directory in `wp-content/themes/`.
- Create a `style.css` with a header that specifies the parent theme:
  ```css
  /*
   Theme Name:   My Child Theme
   Template:     parent-theme-name
  */
  ```
- Create a `functions.php` to enqueue the parent theme’s styles:
  ```php
  function my_child_theme_styles() {
      wp_enqueue_style( 'parent-theme', get_template_directory_uri() . '/style.css' );
      wp_enqueue_style( 'child-theme', get_stylesheet_uri(), array('parent-theme') );
  }
  add_action( 'wp_enqueue_scripts', 'my_child_theme_styles' );
  ```
- Activate the child theme from the WordPress dashboard.

## 6. **Include vs Require**

| **Aspect**     | **Include**                              | **Require**                                |
|----------------|------------------------------------------|--------------------------------------------|
| **Behavior**   | Includes the specified file, gives a warning if the file is not found | Includes the specified file, gives a fatal error if the file is not found |
| **Usage**      | Use when the file is optional            | Use when the file is necessary for the application to run |
| **Error Handling** | Non-fatal error (E_WARNING)            | Fatal error (E_COMPILE_ERROR)              |

## 7. **home_url vs site_url**

- **home_url()**: Returns the home URL, the URL of your WordPress site’s home page (the front-end).
- **site_url()**: Returns the site URL, the URL where WordPress is installed (can be different from `home_url()`).

| **Function** | **Usage**                      | **Example**                          |
|--------------|--------------------------------|--------------------------------------|
| **home_url()**  | Link to the site’s home page | `https://example.com` |
| **site_url()**  | Link to the WordPress installation directory | `https://example.com/wp` |

## 8. **Taxonomy**

- **Taxonomy**: A way to group posts together in WordPress. The two main built-in taxonomies are Categories and Tags.
- Custom taxonomies can be created for custom post types to organize content in different ways.

## 9. **Post Type**

- **Post Types**: Different types of content in WordPress. Default post types include Posts, Pages, Attachments, Revisions, and Nav Menus.
- Custom Post Types can be created to support different types of content, such as Products, Events, or Portfolios.

## 10. **Flexicontent**

- **Flexicontent**: Not a core WordPress feature; however, it refers to flexible content management practices that might involve custom post types, fields, or advanced editors.

## 11. **Page Template**

- **Page Templates**: Custom templates for specific pages in WordPress. These templates can be assigned to pages in the WordPress admin.
- Example of a page template:
  ```php
  <?php
  /*
  Template Name: Custom Page Template
  */
  get_header();
  ?>

  <div class="custom-content">
    <!-- Page content -->
  </div>

  <?php get_footer(); ?>
  ```

## 12. **How to Create a Custom Plugin?**

- Create a directory in `wp-content/plugins/`.
- Create a PHP file with plugin information in the header:
  ```php
  <?php
  /*
  Plugin Name: My Custom Plugin
  Description: A brief description of what the plugin does.
  Version: 1.0
  Author: Your Name
  */
  ```
- Add your custom functionality.
- Activate the plugin from the WordPress admin.

## 13. **Hooks in WordPress**

- **Action Hooks**: Allow you to add custom code at specific points during WordPress execution. Example:
  ```php
  function my_custom_function() {
      echo 'Hello World!';
  }
  add_action( 'wp_footer', 'my_custom_function' );
  ```
- **Filter Hooks**: Allow you to modify data before it is sent to the browser. Example:
  ```php
  function my_custom_title($title) {
      return 'Modified: ' . $title;
  }
  add_filter( 'the_title', 'my_custom_title' );
  ```

## 14. **PHP OOPs Concept**

- **Class**: A blueprint for creating objects.
- **Object**: An instance of a class.
- **Inheritance**: Allows a class to inherit properties and methods from another class.
- **Polymorphism**: Allows methods to do different things based on the object it is acting upon.
- **Encapsulation**: Keeping the internal details of an object hidden.
- **Abstraction**: Hiding complex implementation details and showing only the necessary parts.

## 15. **PHPCS**

- **PHPCS (PHP CodeSniffer)**: A tool to detect violations of a defined coding standard in PHP. Commonly used with WordPress to ensure code adheres to WordPress Coding Standards.
  
## 16. **Do You Use Boilerplate to Create Plugins?**

- Boilerplates are pre-written, structured codebases that help in creating plugins faster with standard practices. Commonly used WordPress plugin boilerplates include the [WordPress Plugin Boilerplate](https://wppb.me/).

## 17. **WordPress Meta**

- **Post Meta**: Custom fields associated with a post, stored in the `wp_postmeta` table.
- **User Meta**: Custom fields associated with a user, stored in the `wp_usermeta` table.
- **Term Meta**: Custom fields associated with a taxonomy term.

## 18. **WordPress REST API**

- **WordPress REST API**: Allows developers to interact with WordPress remotely by sending and receiving JSON objects via HTTP requests. It enables creating, reading, updating, and deleting WordPress content programmatically.
- Example: Fetching posts using REST API:
  ```javascript
  fetch('https://example.com/wp-json/wp/v2/posts')
  .then(response => response.json())
  .then(posts => console.log(posts));
  ```

## 19. **WooCommerce REST API**

- **WooCommerce REST API**: Extends WordPress REST API to handle WooCommerce store data such as products, orders, and customers.
- Example: Fetching products using REST API:
  ```php
  $woocommerce = new Client(
      'https://example.com',
      'ck_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
      'cs_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
      [
          'wp_api' => true,
          'version' => 'wc/v3',
      ]
  );

  $products = $woocommerce->get('products');
  ```

## 20. **WooCommerce Override Template in

 Theme**

- To override a WooCommerce template, copy the template file from `wp-content/plugins/woocommerce/templates/` to `wp-content/themes/your-theme/woocommerce/`, preserving the folder structure.

## 21. **If You Want to Implement CF7 Constraint What Are the Filters?**

- **Contact Form 7 Filters**: Used to modify the behavior of CF7 forms. Examples include:
  - `wpcf7_validate_text`
  - `wpcf7_validate_email`
  - `wpcf7_mail_components`
  ```php
  add_filter('wpcf7_validate_email', 'custom_email_validation_filter', 20, 2);
  function custom_email_validation_filter($result, $tag) {
      // Custom validation logic
      return $result;
  }
  ```

## 22. **What Kind of Work You Did in WooCommerce?**

- Customization of WooCommerce templates.
- Creation of custom product types.
- Integration with third-party APIs for inventory, shipping, and payment processing.
- Performance optimization and scaling for large WooCommerce stores.
- Implementation of custom checkout processes.

## 23. **WooCommerce Shipping and Cart Related Hooks**

- **Shipping Hooks**: `woocommerce_shipping_methods`, `woocommerce_after_shipping_rate`.
- **Cart Hooks**: `woocommerce_before_cart`, `woocommerce_cart_calculate_fees`.
  ```php
  add_action('woocommerce_cart_calculate_fees', 'add_custom_fee');
  function add_custom_fee() {
      global $woocommerce;
      $woocommerce->cart->add_fee( 'Custom Fee', 5 );
  }
  ```

## 24. **WooCommerce Reminder or Abandoned Cart Management**

- **Abandoned Cart Recovery**: Can be implemented using plugins or custom code that tracks cart status and sends reminder emails.
- **Abandoned Cart Management**:To manage Abandoned Cart Management using custom code in WordPress, you can follow these steps:

### 1. **Track Cart Abandonment**
   - Use WooCommerce hooks to detect when a cart is updated and when a user abandons the cart. You can store the cart data and the user’s email address in a custom database table or as user meta.

### 2. **Create a Custom Database Table**
   - Create a custom database table to store abandoned cart data, including user ID, cart contents, and timestamps.

   ```php
   function create_abandoned_cart_table() {
       global $wpdb;
       $table_name = $wpdb->prefix . 'abandoned_carts';
       $charset_collate = $wpdb->get_charset_collate();

       $sql = "CREATE TABLE $table_name (
           id mediumint(9) NOT NULL AUTO_INCREMENT,
           user_id bigint(20) UNSIGNED NOT NULL,
           cart_contents longtext NOT NULL,
           timestamp datetime DEFAULT '0000-00-00 00:00:00' NOT NULL,
           PRIMARY KEY (id)
       ) $charset_collate;";

       require_once(ABSPATH . 'wp-admin/includes/upgrade.php');
       dbDelta($sql);
   }
   add_action('after_setup_theme', 'create_abandoned_cart_table');
   ```

### 3. **Capture Cart Updates**
   - Use the `woocommerce_cart_updated` hook to capture cart updates and store them.

   ```php
   function save_abandoned_cart_data() {
       if (is_user_logged_in()) {
           $user_id = get_current_user_id();
           $cart_contents = WC()->cart->get_cart();
           $cart_contents_serialized = maybe_serialize($cart_contents);

           global $wpdb;
           $table_name = $wpdb->prefix . 'abandoned_carts';

           $wpdb->replace(
               $table_name,
               array(
                   'user_id' => $user_id,
                   'cart_contents' => $cart_contents_serialized,
                   'timestamp' => current_time('mysql'),
               ),
               array(
                   '%d',
                   '%s',
                   '%s'
               )
           );
       }
   }
   add_action('woocommerce_cart_updated', 'save_abandoned_cart_data');
   ```

### 4. **Detect Cart Abandonment**
   - Set up a WP-Cron job to check for abandoned carts periodically.

   ```php
   function check_abandoned_carts() {
       global $wpdb;
       $table_name = $wpdb->prefix . 'abandoned_carts';
       $time_limit = date('Y-m-d H:i:s', strtotime('-1 hour'));

       $results = $wpdb->get_results($wpdb->prepare(
           "SELECT * FROM $table_name WHERE timestamp <= %s",
           $time_limit
       ));

       foreach ($results as $row) {
           $user_info = get_userdata($row->user_id);
           $cart_contents = maybe_unserialize($row->cart_contents);
           send_abandoned_cart_email($user_info->user_email, $cart_contents);
       }
   }

   function schedule_abandoned_cart_cron() {
       if (!wp_next_scheduled('check_abandoned_carts')) {
           wp_schedule_event(time(), 'hourly', 'check_abandoned_carts');
       }
   }
   add_action('wp', 'schedule_abandoned_cart_cron');
   add_action('check_abandoned_carts', 'check_abandoned_carts');
   ```

### 5. **Send Reminder Emails**
   - Create a function to send an email to the user reminding them of their abandoned cart.

   ```php
   function send_abandoned_cart_email($user_email, $cart_contents) {
       $subject = 'You have items waiting in your cart!';
       $message = 'You left some items in your cart. Click here to complete your purchase: ';
       $message .= '<ul>';

       foreach ($cart_contents as $item) {
           $product = wc_get_product($item['product_id']);
           $message .= '<li>' . $product->get_name() . ' - ' . $item['quantity'] . '</li>';
       }

       $message .= '</ul>';

       wp_mail($user_email, $subject, $message);
   }
   ```

### 6. **Clean Up Old Data**
   - Optionally, add a cron job to delete records from the custom table that are older than a certain period (e.g., 30 days).

   ```php
   function clean_up_old_abandoned_carts() {
       global $wpdb;
       $table_name = $wpdb->prefix . 'abandoned_carts';
       $time_limit = date('Y-m-d H:i:s', strtotime('-30 days'));

       $wpdb->query($wpdb->prepare(
           "DELETE FROM $table_name WHERE timestamp <= %s",
           $time_limit
       ));
   }

   add_action('wp_scheduled_delete', 'clean_up_old_abandoned_carts');
   ```

### 7. **Testing and Monitoring**
   - Test the functionality to ensure it’s capturing abandoned carts correctly and sending emails.
   - Monitor the cron jobs and email delivery to make sure everything is functioning as expected.

## 25. **CRON**

- **WP-Cron**: WordPress’s own cron job system, which simulates a system cron. It’s used to schedule tasks like publishing scheduled posts, sending emails, etc.
- Example: Scheduling a task:
  ```php
  if (!wp_next_scheduled('my_custom_cron_job')) {
      wp_schedule_event(time(), 'hourly', 'my_custom_cron_job');
  }
  add_action('my_custom_cron_job', 'my_custom_function');
  function my_custom_function() {
      // Task to run
  }
  ```

## 26. **Plugin Path vs Plugin URL**

| **Aspect**       | **plugin_dir_path()**              | **plugins_url()**                      |
|------------------|-----------------------------------|----------------------------------------|
| **Returns**      | Absolute path to the plugin’s directory | URL to the plugin’s directory |
| **Usage**        | Use when you need a file path     | Use when you need a URL               |

## 27. **WP Config Related**

- **`wp-config.php`**: The configuration file for WordPress that contains database connection details, security keys, and various WordPress settings.
- Important settings:
  - `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`
  - `WP_DEBUG`, `WP_DEBUG_LOG`, `WP_DEBUG_DISPLAY`
  - `WP_MEMORY_LIMIT`

## 28. **PHP Date, String, Array Functions**

- **Date Functions**: `date()`, `strtotime()`, `time()`
- **String Functions**: `strlen()`, `strpos()`, `substr()`
- **Array Functions**: `array_merge()`, `array_filter()`, `array_map()`

## 29. **Magic Functions**

- **Magic Functions**: Special functions in PHP that start with `__` and perform specific tasks.
  - `__construct()`: Called when an object is created.
  - `__destruct()`: Called when an object is destroyed.
  - `__get()`, `__set()`: For accessing and setting inaccessible properties.
  - `__call()`: Invoked when calling inaccessible methods.
  - `__toString()`: Called when an object is treated like a string.

## 30. **Modern PHP Concepts**

- **Namespaces**: Organize code into virtual directories to avoid naming conflicts.
- **Traits**: Reusable methods that can be inserted into classes.
- **Anonymous Classes**: Classes defined without a name.
- **Generators**: Iterators that allow looping through data without needing to load everything into memory.
- **Type Declarations**: Enforcing the type of variables, function arguments, and return types.
