## Ajax Example

### The JavaScript code
You can create a JS file and or copy the below code in a JS file.

```javascript
jQuery(document).ready(function($) {
	"use strict";
	$( ".attorney-letter-filter li.has_content" ).click(function(event) {
		event.preventDefault();
		var the_data = {
			'action': 'pf4_attorney',
			'letter_filter': $(this).attr('data-slug'),
      'search_string': $(this).attr('data-string'),
		};

		$.ajax({
			url: pf4_ajax.ajax_url,
			type : "POST",
			data : the_data,
			dataType: "json",
			success:function(data){  
				$(".attorney-listing .section-inner").html(data.html);
			},
				error:function(xhr){
				alert('Ajax request fail' + xhr);
			}
		});
					
	});
});
```

### The PHP code
You need two pice of code to put in functions.php file

```php
function pf4_scripts() {
	wp_enqueue_script( 'theme-script', get_template_directory_uri() . '/js/custom.js', array('jquery'), '1.0', false );
	/* Adding the ajax link to frontend */
	wp_localize_script( 'theme-script', 'pf4_ajax', array( 'ajax_url' => admin_url( 'admin-ajax.php' ) ) );
}
add_action( 'wp_enqueue_scripts', 'pf4_scripts' );


/*
 The ajax functions
===============================*/
add_action( 'wp_ajax_pf4_attorney', 'pf4_attorney_ajax_request' );
add_action( 'wp_ajax_nopriv_pf4_attorney', 'pf4_attorney_ajax_request' );
function pf4_attorney_ajax_request(){
	$search_string = (isset($_POST['search_string'])) ? $_POST['search_string'] : NULL;
	$letter_filter = (isset($_POST['letter_filter'])) ? $_POST['letter_filter'] : NULL;
	
	ob_start();
		echo $search_string;
    echo $letter_filter;
	$html = ob_get_contents();
	ob_end_clean();	
	
	echo json_encode(array('html' => $html));
	
	wp_die();
}
```