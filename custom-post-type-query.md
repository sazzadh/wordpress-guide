## Custom Post Type Query

```php
$query_args = array(
	'post_type'      => 'resource',
	'posts_per_page' => $limit,
);
	
if ( $cat || $exclude_cat) {
	if ( $cat ) {
		$query_args['tax_query'][] = array(
			'taxonomy' => 'resource_cat',
			'terms'    => explode( ',', $cat ),
			'field'    => 'term_id'
		);
	}

	if ( $exclude_cat ) {
		$query_args['tax_query'][] = array(
			'taxonomy' => 'resource_cat',
			'terms'    => explode( ',', $exclude_cat ),
			'field'    => 'term_id',
			'operator' => 'NOT IN',
		);
	}
}
	
if( ! empty( $ids ) ){
	$query_args['post__in'] = explode( ',', $ids );
}
if( ! empty( $exclude_ids ) ){
	$query_args['post__not_in'] = explode( ',', $exclude_ids );
}
if( ! empty( $offset ) ){
	$query_args['offset'] = $offset;
}
	
if ( get_query_var( 'paged' ) ){
	$query_args['paged'] = get_query_var('paged');
}elseif ( get_query_var( 'page' ) ){
	$query_args['paged'] = get_query_var( 'page' );
}else{
	$query_args['paged'] = 1;
}
	
if($column == '1'){
	$column = '12';
}elseif($column == '2'){
	$column = '6';
}elseif($column == '3'){
	$column = '4';
}elseif($column == '4'){
	$column = '3';
}
	
$the_query = new WP_Query( $query_args );

if ( $the_query->have_posts() ) {
	while ( $the_query->have_posts() ) { $the_query->the_post();
		the_title();			    
	}
	wp_reset_postdata();
}
```