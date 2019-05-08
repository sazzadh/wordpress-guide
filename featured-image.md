## Featured Image

### Get full URl of a featured image

you many need it to use as a background

``php
echo esc_url(wp_get_attachment_url( get_post_thumbnail_id() ));
``