Do you trust all 160 parties in `/etc/ssl/certs` to authenticate your connection to github.com?  If not, you probably want to pin some SSL certificates before cloning.  **pinned-git** does this for github.com, bitbucket.org, code.google.com, and git.gitorious.org.

pinned-git protects only `git clone` right now.

This is pre-alpha software and has only been tested on Ubuntu 13.10.  Do not expect actual security yet.  This would also be better implemented in git itself.
