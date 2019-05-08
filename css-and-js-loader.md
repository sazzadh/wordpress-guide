## Load CSS and JS

### Plugin example 

```php
function PlainPortfolio__scripts() {
	wp_enqueue_style( 'plain-portfolio', plugin_dir_url( __FILE__ ) . 'assets/plain-portfolio.css', array(), '1.0');
	wp_enqueue_script('plain-portfolio', plugin_dir_url( __FILE__ ) . 'assets/plain-portfolio.js', array('jquery'), '1.0', true );
}
add_action( 'wp_enqueue_scripts', 'PlainPortfolio__scripts' );
```

### Theme Example

```php
function glsa_scripts() {
	wp_enqueue_style( 'glsa-common', get_template_directory_uri() . '/css/common.css', array(), '1.0');
	wp_enqueue_style( 'glsa-flexslider', get_template_directory_uri() . '/css/flexslider.css', array(), '1.0');
	wp_enqueue_style( 'glsa-style', get_stylesheet_uri() );
	wp_enqueue_script( 'glsa-script', get_template_directory_uri() . '/js/custom.js', array('jquery'), '1.0', false );
	wp_enqueue_script( 'glsa-flexslider-script', get_template_directory_uri() . '/js/jquery.flexslider-min.js', array('jquery'), '1.0', false );
	if ( is_singular() && comments_open() && get_option( 'thread_comments' ) ) {
		wp_enqueue_script( 'comment-reply' );
	}	
}
add_action( 'wp_enqueue_scripts', 'glsa_scripts' );
```

### Admin Scripts

```php
function glsa_admin_scripts() {
	wp_enqueue_style( 'glsa', get_template_directory_uri() . '/css/admin.css', array(), '1.0');
	wp_enqueue_script( 'glsa', get_template_directory_uri() . '/js/admin.js', array('jquery'), '1.0', false );
}
add_action( 'admin_enqueue_scripts', 'glsa_admin_scripts' );
```