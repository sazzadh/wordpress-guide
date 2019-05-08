## Featured Image

### Get full URl of a featured image

you many need it to use as a background

```php
echo esc_url(wp_get_attachment_url( get_post_thumbnail_id() ));
```

### To show Featured Image in a Post Loop
For More Example [Post_Thumbnails](https://codex.wordpress.org/Post_Thumbnails)

```php
the_post_thumbnail('full'); 
```

There are some size
```php
// without parameter -> Post Thumbnail (as set by theme using set_post_thumbnail_size())
the_post_thumbnail();

the_post_thumbnail('thumbnail');       // Thumbnail (default 150px x 150px max)
the_post_thumbnail('medium');          // Medium resolution (default 300px x 300px max)
the_post_thumbnail('medium_large');    // Medium Large resolution (default 768px x 0px max)
the_post_thumbnail('large');           // Large resolution (default 1024px x 1024px max)
the_post_thumbnail('full');            // Original image resolution (unmodified)

the_post_thumbnail( array(100,100) );  // Other resolutions
```