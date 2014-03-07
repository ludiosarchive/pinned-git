# pinned-git

Do you trust all 160 parties in `/etc/ssl/certs` to authenticate your connection to github.com?  If not, you probably want to pin some SSL certificates before cloning.  **pinned-git** does this for you, for the domains listed below.

**pinned-git** protects only `git clone` right now.

This is pre-alpha software and has only been tested on Ubuntu 13.10.  Do not expect actual security yet.  This would also be better implemented in git itself.


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
