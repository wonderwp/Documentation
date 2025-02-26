# The medias component

So far, the medias component only has an abstract `Medias` class that gathers some useful methods to work with media resources.

List of methods:

- `getIdFromUrl($mediaUrl)` : Retrieve the attachhment id from the attachment url. Useful because most WordPress attachment functions work with an id instead of a file url.

- `mediaAtSize($mediaUrl, $size, $icon = false, $attr = '')` : basically a warapper for wp_get_attachment_image.

- `getFeaturedImage($postId = null, $short = true)` Retrieve the featured image from a particular post.

- `mediaAtSizeFromPostId($postId, $size, $icon = false, $attr = '')`
 similar to mediaAtSize but works directly with a post instead of a file url.

- `uploadTo(array $file = null, $dest = '', $fileName = null)` Allows you to upload a  $_FILE array to a certain directory.