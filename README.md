reviewtool
==========

This document attempts to explain how the reviewtool should work, what its main components are, and what the developer interactions look like.

Basic Architecture
==================

At a high level, reviewtool consists of two components, a VCS hook, and a web based code review tool.  The VCS hook is what the developers will interact with when submitting a set of changes for review, and the web based code review tool is what they interact with when they are performing the actual code review.

Each bug can have one or more reviewsets, which are defined to be one or more list of commits that the developer is proposing as a fix to a given bug.  The author of the changes can submit more than one commit each time if they wish to divide up their work into smaller chunks for ease of review, and they may choose to submit more than one set of commits if they need to revise the original proposed fix for a bug.  Each commit in each item in the reviewset can have a review status, which is either r? (pending review), r- (review not granted) or r+ (review granted).  Reviews with a reviewset containing r+'es for each commit can be closed.  Further review is not possible on closed reviews, and if needed the review can be reopened.  At a future time, the closing and reopening of reviews may be integrated with Bugzilla or other tools.

The VCS Component
=================

The VCS component is what the developer interacts with.  It runs as a push hook on the Mercurial or Git server.  The job of this component is to accept a set of commits, associate them with a bug number, and store them in a database.  In order for the bug number to be pushed, we need an out of band way of specifying that.  The details of that differs between hg and git.  The following examples show how this is used, where <i>remote</i> is a path to the remote server where the hook is installed.

<pre><code>
hg push --remotecmd='export BUG_NUMBER=123456; hg' <i>remote</i>
</code></pre>
