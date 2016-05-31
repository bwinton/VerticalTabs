Commit Message Conventions
==========================
These rules are adapted from [the AngularJS commit conventions](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#commit).

* [Goals](#goals)
* [Generating CHANGELOG.md](#generating-changelogmd)
  * [Recognizing unimportant commits](#recognizing-unimportant-commits)
  * [Provide more information when browsing the history](#provide-more-information-when-browsing-the-history)
* [Format of the commit message](#format-of-the-commit-message)
  * [Subject line](#subject-line)
    * [Allowed `<type>`](#allowed-type)
    * [Allowed `<scope>`](#allowed-scope)
    * [`<subject>` text](#subject-text)
  * [Message body](#message-body)
  * [Message footer](#message-footer)
    * [Breaking changes](#breaking-changes)
    * [Referencing issues](#referencing-issues)
  * [Examples](#examples)

Goals
-----
* allow generating CHANGELOG.md by script
* allow ignoring commits by git bisect (not important commits like formatting)
* provide better information when browsing the history

Generating CHANGELOG.md
-----------------------
We use these three sections in changelog: new features, bug fixes, breaking changes.
This list could be generated by script when doing a release. Along with links to related commits.
Of course you can edit this change log before actual release, but it could generate the skeleton.

List of all subjects (first lines in commit message) since last release:
```bash
git log <last tag>..HEAD --first-parent --pretty=format:%s
```


New features in this release
```bash
git log <last release>..HEAD --first-parent --grep feature:
```

### Recognizing unimportant commits
These are formatting changes (adding/removing spaces/empty lines, indentation), missing semi colons, comments. So when you are looking for some change, you can ignore these commits - no logic change inside this commit.

When bisecting, you can ignore these by:
```bash
git bisect skip $(git rev-list --grep irrelevant <good place> HEAD)
```

### Provide more information when browsing the history
This would add kinda “context” information.
Look at these messages (taken from last few angular’s commits):

* Fix small typo in docs widget (tutorial instructions)
* Fix test for scenario.Application - should remove old iframe
* docs - various doc fixes
* docs - stripping extra new lines
* Replaced double line break with single when text is fetched from Google
* Added support for properties in documentation


All of these messages try to specify where is the change. But they don’t share any convention...

---

Format of the commit message
----------------------------
```markdown
<type>: <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

Any line of the commit message cannot be longer 100 characters! This allows the message to be easier to read on github as well as in various git tools.

### Subject line
Subject line contains succinct description of the change.

#### Allowed `<type>`
* feat (feature)
* fix (bug fix)
* docs (documentation)
* style (formatting, missing semi colons, …)
* refactor
* test (when adding missing tests)
* chore (maintain)

#### `<subject>` text
* Use imperative, present tense: “change” not “changed” nor “changes”.
* Capitalize first letter.
* Dot (.) at the end.

### Message body
* Just as in <subject> use imperative, present tense: “change” not “changed” nor “changes”.
* Include motivation for the change and contrast with previous behavior.

http://365git.tumblr.com/post/3308646748/writing-git-commit-messages
http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

### Message footer

#### Breaking changes

All breaking changes have to be mentioned in footer with the description of the change, justification and migration notes

```markdown
BREAKING CHANGE: isolate scope bindings definition has changed and
    the inject option for the directive controller injection was removed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
      myBind: 'bind',
      myExpression: 'expression',
      myEval: 'evaluate',
      myAccessor: 'accessor'
    }

    After:

    scope: {
      myAttr: '@',
      myBind: '@',
      myExpression: '&',
      // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
      myAccessor: '=' // in directive's template change myAccessor() to myAccessor
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

#### Referencing issues

Closed bugs should be listed on a separate line in the footer prefixed with "Fixes" keyword like this:
```markdown
Fixes #234
```

or in case of multiple issues:
```markdown
Fixes #123.
Fixes #245.
Fixes #992.
```

#### Merge commits.

Merges from pull requests should also be mentioned here.
```markdown
Merge pull request #123 from contributor/branch-name
```

Examples
--------
```markdown
fix: Remove margin from popups.

Make the CSS more specific, to handle more cases.

Fixes #135.
Merge pull request #314 from ericawright/remove-from-popups.
```

```markdown
fix: Don't show unidentified buttons in the tab's toolbar.

Style tools from tab-bar as hidden until further decision can be made on where to put them.
Restore tools' positions upon disabling tab center.

Fixes #4.
Fixes #304.
Merge pull request #311 from bwinton/tab-bar-buttons.
```

```markdown
docs: Added a CONTRIBUTING.md.
```