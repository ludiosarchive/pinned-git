Notes:

For certs issued by good CAs (i.e. not COMODO), instead of adding the cert for
e.g. github, we add the cert for github's CA, to reduce the number of updates
we have to make to pinned-git.

If two sites have the same CA, keep them in different filenames away,
because if they switch to different CAs, we'd have to update a bunch of
.git/config files to point to the right cert bundle.
