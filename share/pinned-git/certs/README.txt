Note: if two sites have the same CA, keep the certs in different files anyway,
because if they switch to different CAs, we'd have to update many .git/config
files to point to the right cert bundle.
