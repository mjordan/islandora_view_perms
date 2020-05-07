## Overview

This module provides proof of concept code for the idea described in https://github.com/Islandora/documentation/issues/413#issuecomment-623001821. Not for production use.

The idea is that we can infer whether a user is authorized to view a file, and the media that wraps that file, based on whether the user is authorized to view the node the media is associated with. Put another way, if a user is authorized to view a node, we can assume that they are also authorized to view any media associated with the node and those media's files. 
If the user is not authorized to view a node, we can assume they are also not authorized to view the meida associated with the node and the file that is associated with the media.

This module demonstrates a method for inferring authorization on nodes, media, and files. Of course there are use cases that do not adhere to these assumptions, but this proof of concept is intended only to demonstrate that we can infer view authorization from node all the way down to file.

Inferring authorization could be extended upward from node to parent node, including parent nodes that are compound objects/books/newspaper issue or collection objects.

## Requirements and installation

No requirements other than files must be stored on Drupal's private file system. Installation is the same as for any other Drupal 8 module.

## How to test

1. Configure Drupal's private file system and make it your site's default. https://www.ostraining.com/blog/drupal/creating-drupal-8-private-file-system/ provides clear, simple documentation on how to do this.
1. Out of the box, derivatives created by Islandora are saved to the public file system. Drupal will need to be configured so that Islandora derivatives are saved to the private file system. For this test, we'll do this for Image service file derivatives. To do this, go to `admin/config/system/actions/configure/image_generate_a_service_file_from_an_original_file?destination=/admin/config/system/actions` and choose "private" file system.
1. Install this module and enable it. There are no configuration settings.
1. Configure Drupal such that it disallows the Anonymous user from viewing nodes. For example, this can be done using the [Permissions by Term](https://www.drupal.org/project/permissions_by_term) contrib module, or by simply removing the Anonymous user's "View published content" permission at `admin/people/permissions`. Inferring view authorization on media and files should work with any of Drupal's access control mechanisms.
1. Create an Repository Item node (of model Image for this test), and add an Image media and PNG or JPEG file.
1. Clear Drupal's cache.

Now, everything is in place to attempt to view a node, media, and file as Anonymous.

1. As the admin user, get the node's URL (e.g. `http://localhost:8000/node/2`), the "original file" media's URL (e.g., `http://localhost:8000/media/9`), and the direct-to-image URL of the "Service file" media (e.g., `http://localhost:8000/system/files/2020-05/2-Service%20File.jpg`). You can get the service file image's URL by viewing the node, right-clicking over the image, and choosing whatever context menu item your browser uses to provide the image URL.
1. Copy and paste these URLs somewhere for use in the final two steps.
1. Go into a private/incognito tab in your browser to start a new user session.
1. As the Anonymous user (i.e., not logged into Drupal), visit all three of the URLs you copied. You should not be allowed to view any of them.


