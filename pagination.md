## Pagination 

### The Function that need to copy in `functions.php` file

```php
function glsa_paginate($query = NULL){
	global $wp_query; 
	if( $query == NULL ){ $query = $wp_query; }
	$output = null;
	if ($query->max_num_pages > 1) {
		$output .= '<div class="pagenav">';
			$big = 999999999; // need an unlikely integer		
			$output .= paginate_links( array(
				'base' => str_replace( $big, '%#%', esc_url( get_pagenum_link( $big ) ) ),
				'format' => '?paged=%#%',
				'current' => max( 1, get_query_var('paged') ),
				'total' => $query->max_num_pages
			));
		$output .= '</div>';
	}
	return $output;
}
```

### The CSS Code

```css
.pagenav{
	display: block;
	text-align: center;
	padding-top:31px;
}
.pagenav .page-numbers{
	padding: 6px 16px;
	border: solid 1px rgba(255, 255, 255, 0);
	display: inline-block;
	text-decoration: none;
	text-transform: uppercase;
	font-size: 14px;
	font-weight: bold;
}
.pagenav .page-numbers:hover{
	border-color: #D6A347;
}
.pagenav .page-numbers.current{
	color: #D6A347;
}
```

### Use example
Standerd WordPress loop. Put it right after the loop close

```php
if(function_exists('glsa_paginate')){
	glsa_paginate($query = NULL);
}
```

Custom Query Loop WordPress loop. Put it right after the loop close

```php
$the_query = new WP_Query( $args );

if(function_exists('glsa_paginate')){
	glsa_paginate($the_query);
}
```