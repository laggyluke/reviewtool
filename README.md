Changeset-Based Review Tool (CBRT)
==================================

This document attempts to explain how the CBRT should work, what its main components are, and what the developer interactions look like.

10,000 Feet Overview
====================

Node.JS app - API and UI on top of it

Review tool:
* offer an api for git/hg hooks to notify tool of new reviews: return url of a view to set r? requests(1 url per bug)
* url shows a list of all changesets pushed that triggered the hook and allow dev to set r? on each changeset pushed
* call bugzilla api to update relevant bugs with review activity
* changesets should be listed in attachments view like patches are now
* lets developers annotate(append-only in the backend) lines in relevant changesets with comments (both before and after the change, in case someone needs to say something about the way the code used to work before the change.)
* can set overall review to "r+ accepted" or "r- rejected" or "r? cleared" or reassign r? to someone else
* syntax highlight would be nice, but not necessary yet(might be able to reuse hg browsing ui or dxr)
* links pointing back from bugzilla should be persistent(should keep hg/git changesets forever)
* any changes in UI are discardable until user presses 'commit'. At this point changes should be synced with bugzilla and persisted (add comments, set the appropriate flags on the attachment in bugzilla too)

Post-review?
* offer the ability to set a checkin? flag on a set of changesets pushed through the tool, specifying which branch they need to be merged into (try/inbound/etc + a destination sha1 which would be tip for inbound, and some other sha1 for try pushes).
* offer an API to pull the sha1 for the topmost commit in the set of changes which have a checkin? flag
* the autoland tool would look for checkin? flags, and pulls those changes and transplants them on top the branch in question using the destination sha1
* for git pushes, the git commits need to be converted to hg changesets using hg-git. this part can happen later
* if the changesets are not transplanted cleanly, the author can rebase their work locally, push it again to the review tool, and set the checkin? flag on the rebased set of changes
* when a transplant successfully finishes, the autoland tool will set the checkin+ flag through an API, otherwise it will set the checkin- flag
* this lets people deal with pushing multiple changes from multiple bugs atomically by making sure that all of those changes exist in their local repos, which integrates well with the local workflow


Basic Architecture
==================

At a high level, CBRT consists of two components, a VCS hook, and a web based code review tool.  The VCS hook is what the developers will interact with when submitting a set of changes for review, and the web based code review tool is what they interact with when they are performing the actual code review.

Each bug can have one or more reviewsets, which are defined to be one or more list of commits that the developer is proposing as a fix to a given bug.  The author of the changes can submit more than one commit each time if they wish to divide up their work into smaller chunks for ease of review, and they may choose to submit more than one set of commits if they need to revise the original proposed fix for a bug.  Each commit in each item in the reviewset can have a review status, which is either r? (pending review), r- (review not granted) or r+ (review granted).  Reviews with a reviewset containing r+'es for each commit can be closed.  Further review is not possible on closed reviews, and if needed the review can be reopened.  At a future time, the closing and reopening of reviews may be integrated with Bugzilla or other tools.

The VCS Component
=================

The VCS component is what the developer interacts with.  It runs as a push hook on the Mercurial or Git server.  The job of this component is to accept a set of commits, associate them with a bug number, and store them in a database.  In order for the bug number to be pushed, we need an out of band way of specifying that.  The details of that differs between hg and git.  The following examples show how this is used, where <i>remote</i> is a path to the remote server where the hook is installed.

<pre><code>hg push -f --remotecmd='export BUG_NUMBER=123456; hg' <i>remote</i></code></pre>
<pre><code>git push -f <i>remote</i> master:bug/123456</code></pre>

(Note that there might be better ways of passing the bug number in Mercurial.)

Once the server side VCS component records the pushed commits, it outputs a URL which will be logged on the stdout of the hg/git command, and clicking that link takes the author to the review page for that bug where they can see the commits that they have just pushed.  On that page, for any commit which was accepted on the server VCS, the author can set r? and select a reviewer based on their name and email address.  This is basically how the tool figures out which commits form what has been pushed are considered as part of the review and which ones should be ignored.  The commits that have already been pushed somewhere else (such as mozilla-central for example) will be implicitly ignored here.
