## Ajax Post Load More

### The JavaScript code
You can create a JS file and or copy the below code in a JS file.

```javascript
/*
	CUstom Post type Load More 
-------------------------------*/
jQuery(document).ready(function($) {
	"use strict";
	$('body').on( "click", '#neutrals_loadMore_link', function(event) {
		event.preventDefault();
		var the_data = {
			'action': 'neutrals_loadmore',
      		'current_page_num': $(this).attr('data-current-page-num'),
      		'total_page_num': $(this).attr('data-total-page-num')
		};

		$.ajax({
			url: pf4_ajax.ajax_url,
			type : "POST",
			data : the_data,
			dataType: "json",
			success:function(data){  
				$("#neutrals_post_list_nojs").append(data.html);
				$("#neutrals_loadMore_link").attr('data-current-page-num', data.next_page_num);
				if(data.is_future_page === 'no'){
					$(".neutrals_loadMore").css('display', 'none');
				}
			},
				error:function(xhr){
				alert('Ajax request fail' + xhr);
			}
		});
				
	});
});
```

### The PHP code for Functions.php file
You need two pice of code to put in functions.php file

```php
function pf4_scripts() {
	wp_enqueue_script( 'theme-script', get_template_directory_uri() . '/js/custom.js', array('jquery'), '1.0', false );
	/* Adding the ajax link to frontend */
	wp_localize_script( 'theme-script', 'pf4_ajax', array( 'ajax_url' => admin_url( 'admin-ajax.php' ) ) );
}
add_action( 'wp_enqueue_scripts', 'pf4_scripts' );


/*
 The ajax functions for custom post type load more
===============================*/
add_action( 'wp_ajax_neutrals_loadmore', 'pf4_neutrals_loadmore_ajax_request' );
add_action( 'wp_ajax_nopriv_neutrals_loadmore', 'pf4_neutrals_loadmore_ajax_request' );
function pf4_neutrals_loadmore_ajax_request(){
	$current_page_num = (isset($_POST['current_page_num'])) ? $_POST['current_page_num'] : NULL;
	$total_page_num = (isset($_POST['total_page_num'])) ? $_POST['total_page_num'] : NULL;

	if($current_page_num < $total_page_num){
		$next_page_num = ($current_page_num + 1);
	}else{
		$next_page_num = NULL;
	}

	$is_future_page = 'yes';
	$future_page_num = ($next_page_num + 1);
	if($future_page_num > $total_page_num){
		$is_future_page = 'no';
	}

	$neutrals = new WP_Query( array( 'post_type' => 'neutrals', 'posts_per_page' => 2, 'paged' => $next_page_num ) );	
	ob_start();
		while ( $neutrals->have_posts() ) : $neutrals->the_post();
			//Normal Post HTML code for title, link etc
		endwhile; 
		wp_reset_query();
	$html = ob_get_contents();
	ob_end_clean();	
	
	echo json_encode(array('html' => $html, 'next_page_num' => $next_page_num, 'is_future_page' => $is_future_page));
	
	wp_die();
}
```

### The PHP code for Shortcode or the Page Templates

```php
<div class="neutrals_post_lists" id="neutrals_post_list_nojs">
	<?php $neutrals = new WP_Query( array( 'post_type' => 'neutrals', 'posts_per_page' => 2 ) ); ?>
	<?php while ( $neutrals->have_posts() ) : $neutrals->the_post(); ?>
		<!--//Normal Post HTML code for title, link etc-->
	<?php endwhile; wp_reset_query(); ?>
</div>
<div class="neutrals_loadMore">
	<a href="#" data-current-page-num="<?php echo max( 1, get_query_var('paged') ); ?>" data-total-page-num="<?php echo $neutrals->max_num_pages; ?>" id="neutrals_loadMore_link">VIEW MORE +</a>
</div>
```