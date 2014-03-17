Integration Options
===================

### Note

We are NOT considering moving away from Bugzilla for issue tracking.


Review Board
------------

Review Board would use a separate review repository.

Review request can be attached to Bugzilla bug (manually or automatically),
so the status of review request is visible from inside the Bugzilla.


### Blockers

* 7 - AFAIK there's no way to have a persistent URL corresponding to a single
    Feature in the current version of RB.
* 16 - scaling issues needs further investigation.
* 17 - preserving the history of all review requests depends on 16, may be tricky.
* 27 - related to 7, there's no easy way to transform "single changeset feature"
    to "multiple changesets feature" in RB, because it has no concept of
    feature branches or something similar.


Bitbucket
---------

The primary repository (mozilla-central) is hosted on Bitbucket.

Both mozillians and external contributors will have to fork the repository
and submit pull requests.

Pull request can be attached to Bugzilla bug (manually or automatically),
so the status of pull request is visible from inside the Bugzilla.

Tradeoffs: Bitbucket lacks some nice features of GitHub
(e.g. [Commit Status API](https://github.com/blog/1227-commit-status-api)),
but we don't have to migrate from Mercurial to Git.

Note: AFAIK there's no GitLab equivalent for Bitbucket (see below).

### Blockers

* 3 - user can submit pull requests without specifying Bugzilla bug number.
    There are ways to work around this, but they are mostly user-hostile.
* 16 - scaling issues needs further investigation.
* 19 - repository must be hosted at `bitbucket.org`.
* 23 - can't ignore individual commits when creating a pull request.
    User will have to do some history rewriting on her own.
* 26 - repository must be hosted at `bitbucket.org`.



GitHub
------

The primary repository (mozilla-central) is hosted on GitHub.

Mozillians can create feature branches and submit pull requests.
External contributors can fork the repository and submit pull requests.

Pull request can be attached to Bugzilla bug (manually or automatically),
so the status of pull request is visible from inside the Bugzilla.

Tradeoff: Repository has to be migrated from Mercurial to Git,
but in return we're lowering the barriers for new contributors.

### Blockers

* 3 - user can submit pull requests without specifying Bugzilla bug number.
    There are ways to work around this, but they are mostly user-hostile.
* 16 - scaling issues needs further investigation, but personally I'm more
    confident with Git.
* 19 - unless Mozilla is going to use the hosted version of GitHub Enterprise,
    the repository won't be hosted at `<something>.mozilla.org`,
    but at `github.com`.
* 21 - this basically means migrating from Mercurial to Git.
* 23 - can't ignore individual commits when creating a pull request.
    User will have to do some history rewriting on her own.
* 25 - `mozilla-central` will have to be migrated to Git.
* 26 - `mozilla-central` won't be hosted on `hg.mozilla.org` anymore.


GitLab
------

Self-hosted open source clone of GitHub implementing a limited subset of
features.

Should be evaluated separately.

