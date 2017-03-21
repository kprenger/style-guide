# Github style guide

As Github users, we want to ensure our commit messages and PR requests follow similar guidelines across all work. For process specifics, refer to our Confluence doc.

## Table of Contents

* [Branch Names](#branch-names)
* [Commit Messages](#commit-messages)
* [Pull Requests](#pull-requests)

## Branch Names

Branch names follow this convention `<ticket type>/<ticket number>-<description (if preferred)>`. The `<ticket type>` refers to an Epic, Story, Bug, etc; the `<ticket number>` refers to the JIRA ticket number; and the `<description>` is an optional description of what is contained in the branch.

```bash
feature/OVODEV-1234-new-feature-thing
feature/OVODEV-8675
bugfix/OVODEV-9876-cannot-use-app
epic/OVODEV-5555-new-screen-stuff
```

## Commit Messages

In general, use correct-ish grammar and punctuation. We aren't going to go crazy enforcing this, but try to refrain from things like `lol made sum codez kthxbai`. Funny commit messages, while hilarious, usually aren't helpful for future us.

All commit messages should be prepended with the JIRA ticket number. This keeps master's commit history organized once commits get pulled in. It allows for anyone to easily see what ticket a commit was made for.

```bash
git commit -m "PROJ-123: I made a thing do another thing"
```

If you aren't working on a JIRA ticket, consider what you're doing and maybe make a JIRA ticket for it. If you absolutely do not need a JIRA ticket, that's fine - just make a commit without a ticket number.

## Pull Requests

A PR title should be a quick summary of the work you did. More details can be added in the description field of your PR. 

Similar to commit messages, make sure to include your JIRA ticket number in the PR title.

```
PROJ-123:  Making a thing do a thing.
```
