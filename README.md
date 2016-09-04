# pinned-git

When git clones a repository, it trusts all 160 parties in `/etc/ssl/certs`, even when a much more limited set of CAs would almost eliminate the risk of MITM.  (To illustrate the risk, we *know* that WoSign already issued an unauthorized cert for github.com).  pinned-git wraps git to automatically pin the right CA certificate for the domain you're cloning from.  pinned-git supports pinning for the [domains listed below](#pinned-domain-list).  If there is no pin for the domain being cloned from, it will proceed anyway without a pin.

pinned-git protects `git clone` and subsequent fetches in the cloned repo, assuming that you keep intact the `http.sslCAInfo` **and** `http.sslCAPath` options in the clone's `.git/config`.  Note that git doesn't support a per-remote `http.sslCAInfo`, so if you have two `https://` remotes on different domains, you may have to remove the pinning or create your own certificate bundle.

Because `sslCAInfo = ` is set to an absolute path, you must install pinned-git to a location that will not change.

pinned-git is more convenient than setting up "anonymous" cloning over the SSH protocol, because you do not have to install and maintain {GitHub, GitLab, ...}-specific SSH keys and configuration on many users/machines.


## Requirements

*	git 1.7.7 or newer.  1.7.7 is the first version that supports `git clone --config ...`


## Pinned domain list

* https://github.com/
* https://gitlab.com/
