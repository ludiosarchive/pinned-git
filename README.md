# pinned-git

[![Build status][travis-image]][travis-url]

When `git` fetches a repository, it trusts all ~173 parties in `/etc/ssl/certs`, even when a more limited set of CAs would almost eliminate the risk of MITM.  To illustrate the risk, we *know* that [WoSign already issued an unauthorized cert for github.com](https://www.schrauger.com/the-story-of-how-wosign-gave-me-an-ssl-certificate-for-github-com).  `pinned-git` wraps `git` to automatically pin the appropriate CA certificate for the domain you're cloning from.  It currently pins **github.com**, **gitlab.com**, **anonscm.debian.org**, **git.kernel.org**, and **repo.or.cz** (note: repo.or.cz is not signed by a trusted root, yet `pinned-git` makes [`https://repo.or.cz/`](https://repo.or.cz/) work without error).  If there is no pin for the domain being cloned from, it will proceed anyway without a pin.

`pinned-git` protects `git clone` and subsequent fetches in the cloned repo, assuming that you keep intact the `http.sslcainfo` **and** `http.sslcapath` options in the clone's `.git/config`.  Note that git doesn't support a per-remote `http.sslcainfo`, so if you have two `https://` remotes on different domains, you will have to remove the pinning or create your own certificate bundle.

Because `sslcainfo = ` is set to an absolute path, you must install pinned-git to a location that will not change.

pinned-git is more convenient than setting up "anonymous" cloning over the SSH protocol, because you do not have to install and maintain {GitHub, GitLab, ...}-specific SSH keys and configuration on many users/machines.  Instead, just one pinned-git install needs to be kept up to date on each machine.


## Requirements

*	git 1.7.7 or newer.  1.7.7 is the first version that supports `git clone --config ...`


[travis-image]: https://img.shields.io/travis/ludios/pinned-git.svg
[travis-url]: https://travis-ci.org/ludios/pinned-git
