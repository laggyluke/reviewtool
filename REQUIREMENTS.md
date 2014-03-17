Requirements
============

See [GLOSSARY](GLOSSARY.md).

1. In order to submit a **Feature** for review, user SHOULD NOT need to install any additional software beyond Mercurial client.
2. User MUST be able to submit her **Feature** for review by issuing a single CLI command.
3. User MUST provide a Bugzilla bug number when submitting her **Feature** for review.
4. User MUST be able to assign a reviewer for **Review Request**.
5. **Code Review Tool** MAY automatically suggest a reviewer.
6. Review Request URL MUST be automatically attached to a corresponding Bugzilla bug.
7. Review Request URL SHOULD NOT change when **Review Request** is later updated with new **Changesets**.
8. Reviewer MUST be able to see individual **Changesets** of a **Review Request**.
9. Reviewer MUST be able to comment on idividual lines of **Changesets**.
10. Reviewer MUST be able to comment on a whole **Feature**.
11. Reviewer SHOULD be able to see a squashed diff of a **Review Request**.
12. Reviewer MUST be able to mark **Review Request** as reviewed.
13. Reviewer MAY be able to mark **Review Request** as super-reviewed.
14. **Review Request** status MUST be visible and up-to-date in a corresponding Bugzilla bug.
15. **Code Review Tool** SHOULD provide syntax highlighting.
16. **Code Review Tool** MUST be able to handle tens of thousands of **Review Requests** during it's lifetime.
17. **Code Review Tool** MUST preserve a history of past **Review Requests**, their **Changests**, comments etc.
18. **Code Review Tool** MUST provide an HTTP API for all its features.
19. **Code Review Tool** SHOULD be hosted at `<something>.mozilla.org`.
20. **Code Review Tool** MAY eventually be a part of a bigger system related to continuous integration.
21. **Code Review Tool** MAY NOT rely on Mercurial features that are not used by most of the Mozillians, e.g. bookmarks and MQ.
22. **Feature** MAY contain **Changesets** that should be ignored during review.
23. User SHOULD be able to ignore such **Changesets** when creating **Review Request**
24. Previously ignored **Changesets** SHOULD be available for review later as a part of a separate **Review Request**.
25. The mozilla-central repository MUST remain hosted on hg.mozilla.org.
26. The mozilla-central repository MUST remain a Mercurial repository, not Git.
27. Review Request MUST be automatically updated when new Changesets are pushed for the corresponding Feature.
