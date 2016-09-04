# pinned-git

Do you trust all 160 parties in `/etc/ssl/certs` to identify only the real GitHub as github.com?  If not, you probably want to pin some SSL certificates, to make sure no men in the middle can modify your git fetch traffic in transit.  git doesn't support this, but pinned-git will automatically pin the right certificate for the domain you're cloning from.  pinned-git supports pinning for the [domains listed below](#pinned-domain-list).  If there is no pin for the domain being cloned from, it will proceed anyway without a pin.

pinned-git protects `git clone` and subsequent fetches in the cloned repo, assuming that you keep intact the `http.sslcainfo` **and** `http.sslcapath` options in the clone's `.git/config`.  Note that git doesn't support a per-remote `http.sslcainfo`, so if you have two `https://` remotes on different domains, you may have to remove the pinning or create your own certificate bundle.

Because `sslcainfo = ` is set to an absolute path, you should install pinned-git to a location that will not change.


## Requirements

*	git 1.7.7 or newer.  1.7.7 is the first version that supports `git clone --config ...`

	*	CentOS 6.5 ships git 1.7.1, Debian 6 ships git 1.7.2.5; almost everything else should have a new-enough git.

*	**git must be linked to GnuTLS**, not OpenSSL or NSS.

	*	You have this if you're using the git package on Debian or Ubuntu.

	*	You don't have this if you're using the git package on CentOS, Fedora, OS X, or Cygwin.

	*	You can run `ldd` on `git-remote-https`; it should not be linked to `libcrypto.so.*`.

	*	Why doesn't pinned-git work with a git linked to OpenSSL?

		For some sites/CAs (gitlab.com/Comodo), pinned-git puts non-CA certs in a CA bundle and tells git/curl to use the right bundle for a specific site.  This works fine with GnuTLS: it doesn't insist on validating the entire certificate chain if we trust the website's cert.

		That doesn't work with OpenSSL, though.  For non-self-signed certificates, OpenSSL requires that we provide enough of a certificate chain in our CA bundle to reach a trusted root CA.  If we provide just just the website's (non-self-signed) cert, `X509_verify_cert` fails with `X509_V_ERR_UNABLE_TO_GET_ISSUER_CERT_LOCALLY` ("SSL certificate problem: unable to get local issuer certificate").  This happens whether or not it's been marked with a trusted bit using `openssl x509 -trustout`.  ["Future versions of OpenSSL will recognize trust settings on any certificate: not just root CAs."](https://www.openssl.org/docs/apps/x509.html#TRUST_SETTINGS)

		If we included a root CA certificate in our CA bundle, we would only get CA pinning, not pinning to the exact certificate the site provides.  CA pinning is better than nothing, but for some CAs (e.g. Comodo, used by gitlab.com), this reduced security level is unacceptable.


## SSH-based alternatives for fetching without trusting CAs

### GitHub

If GitHub knows about your public SSH key, it will provide read-only SSH access to any public repository, not just repositories you can write to.  For example, instead of

```
git clone https://github.com/ludios/Desktopmagic
```

you can use

```
git clone git@github.com:ludios/Desktopmagic
```

Make sure you get the right key from GitHub.  See [their page listing their SSH key fingerprints](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints).

### BitBucket

BitBucket will also provide read-only SSH access to any public repository, as long as they know your public SSH key.  For example, instead of

```
git clone https://bitbucket.org/jcreenaune/svn-git-mirror
```

you can use

```
git clone git@bitbucket.org:jcreenaune/svn-git-mirror
```

[BitBucket's fingerprints are listed here](https://confluence.atlassian.com/display/BITBUCKET/Use+the+SSH+protocol+with+Bitbucket#UsetheSSHprotocolwithBitbucket-KnownhostorBitbucket'spublickeyfingerprints).


## Pinned domain list

* https://github.com/
* https://gitlab.com/
