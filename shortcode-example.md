## Shortcode Example

```php
add_shortcode('shortcode_name', 'shortcode_function_name');
function shortcode_function_name( $atts, $content = null ) {
	extract( shortcode_atts( array(
			'category'         => '',
			'exclude_category' => '',
			'limit'            => 12,
		), $atts)
	);
	
	ob_start();
	echo 'any content put here';
	$output = ob_get_contents();
	ob_end_clean();
	
	return 	$output;
}
```