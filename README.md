# pinned-git

Do you trust all 160 parties in `/etc/ssl/certs` to identify only the real GitHub as github.com?  If not, you probably want to pin some SSL certificates, to make sure no men in the middle can modify your git fetch traffic in transit.  git doesn't support this, but pinned-git will automatically pin the right certificate for the domain you're cloning from.  pinned-git supports pinning for the domains listed below.  If there is no pin for the domain being cloned from, it will proceed anyway without a pin.

pinned-git protects `git clone` and subsequent fetches in the cloned repo, assuming that you keep the `[http] sslcainfo = ` option in the clone's `~/.git/config`.

Because `sslcainfo = ` is set to an absolute path, you should install pinned-git to a location that will not change.

Beware, this is alpha-quality software!


## Requirements

*	git 1.7.7 or newer, the first version with `git clone --config ...`

	*	CentOS 6.5 ships 1.7.1; almost everything else should have a new-enough git.

*	**git must be linked to GnuTLS**, not OpenSSL or NSS.

	*	You have this if you're using the git package on Debian or Ubuntu.

	*	You don't have this if you're using the git package on CentOS, Fedora, or Cygwin.

	*	You can run `ldd` on `git-remote-https`; it should not be linked to `libcrypto.so.*`.

	*	Why doesn't pinned-git work with a git linked to OpenSSL?

		pinned-git puts non-CA certs in a CA bundle and tells git/curl to use the right bundle for a specific site.  This works fine with GnuTLS: it doesn't insist on validating the entire certificate chain if we trust the website's cert.

		For non-self-signed certificates, OpenSSL requires that we provide enough of a certificate chain in our CA bundle to reach a trusted root CA.  If we provide just just the website's (non-self-signed) cert, `X509_verify_cert` fails with `X509_V_ERR_UNABLE_TO_GET_ISSUER_CERT_LOCALLY` ("SSL certificate problem: unable to get local issuer certificate")

		If we do provide a CA certificate in our CA bundle, we can only get CA pinning, not pinning to the exact certificate the site provides.  CA pinning is better than nothing, but we're not interested in providing this lower level of security.


## Pinned domain list

* https://github.com/
* https://bitbucket.org/
* https://code.google.com/
* https://git.gitorious.org/
* https://git.kernel.org/
* https://kernel.googlesource.com/
* https://chromium.googlesource.com/
* https://android.googlesource.com/
* https://git.overlays.gentoo.org/
* https://alioth.debian.org/
* https://git.eclipse.org/
* https://git.gnome.org/
* https://gitlab.com/
