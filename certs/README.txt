Notes:

For certs issued by good CAs (i.e. not COMODO), instead of adding the cert for
the site (e.g. github), we add the cert for github's CA, to reduce the number
of updates we have to make to pinned-git.

Also, if two sites have the same CA or cert, keep them in different files anyway,
because if they switch to different CAs, we'd have to update a bunch of
.git/config files to point to the right cert bundle.


CA or site cert?

github.com - CA
gitlab.com - site
