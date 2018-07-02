Drupal + Docker4Windows Permission issue
========================================

This site exists to reproduce the docker file sharing issues on windows. 

## Issue scope

1. All file system _checks_ that require a non root user to have write access fail
1. All actions that need to adhere to specific permissions or ownership (for example only owner may read settings.php) fail

## Reproducing issue 1.
The drupal core _includes/file.inc_ file containes a function ```file_prepare_directory```. This function is called on each file upload. It creates a directory if it does not yet exists and asumes its writable (as we where able to create a directory). When a file upload is done to a directory that does exist the php build-in function ```is_writable``` is called. This will alwais fail for non root users as the container reports 755 permissions.

### Steps

1. lando start
1. [go to the site and log on](http://reproduce.lndo.site/user)
    * username: admin
    * password: admin
1. [got to add article page](http://reproduce.lndo.site/node/add/article)
1. Add an image file in the image section
1. Add another image file in the image section