# Course Feedback Guidelines: Bugs & Issues

At the end of each lesson, if you discover:  

- **Bugs & Hotfixes**: If there are any bugs with the content (typos, syntax issues with Python code, incorrect definitions, etc), please submit a __Pull Request__ with a brief description of the bug.

- **Missing Content**: If students had difficulty with lesson concepts due to missing or incomplete content gaps, please log an issue in Github.

- **Content Suggestions**: If you want to make a content suggestion for future versions of the curriculum, please log an issue in GitHub.

---

#  Course Construction Guidelines: Submitting New Material

### Pull Requests

All PRs should be submitted from a feature branch on your local fork, straight to *generalassembly/wdi/master*.

Before submitting a pull request, please make sure your local feature branch is up to date with generalassembly/wdi/master:

    $ git remote add upstream git@github.com:generalassembly-studio/dat-curriculum
    $ git fetch --all
    $ git checkout -b my-feature-branch upstream/master

### Lesson, project, and homework branches

If you're submitting a new _lesson_, _lab_, or _homework_ resource, **create a feature branch** for each individual resource.

Lesson branch naming should follow the same naming style & convention we use for folders. For example:

```
$ git checkout master
On branch master
nothing to commit, working directory clean

$ git checkout -b intro-to-relational-data-modeling
Switched to a new branch intro-to-relational-data-modeling
```

If a lesson/lab/homework both have the exact same name, just denote which with `-lesson` or `-lab`.

```
$ git checkout -b exploratory-data-analysis-lesson

- - or - -

$ git checkout -b exploratory-data-analysis-lesson
```

If submitting a new resource, please also make sure to:

- Follow our [styleguide](templates/styleguide.md), with appropriate YAML frontmatter data
- Use our standard [lesson](templates/template-lesson-readme.md) and [project](templates/template-project-readme.md) templates.
