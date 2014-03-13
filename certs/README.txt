Why duplicate copies of cert bundles?

kernel.googlesource.com and android.googlesource.com could start serving
different certs; if pinned-git created repos with http.sslcainfo pointing
to the same file, users would have to manually fix .git/config instead of
just updating pinned-git.


Why two Google certs in the *.google.com.crt files?

Cert #1 was captured with openssl s_client, version 1.0.1e; it received a 2048 bit RSA key
Cert #2 was captured with Firefox 27 on Windows; it received a 256-bit ECDSA key
