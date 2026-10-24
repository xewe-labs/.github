# GitHub files

GitHub reads these paths by their fixed names, so they keep them even though some break the
uppercase rule for Markdown names:

- `profile/README.md` is the organization's landing page. It is a profile page, so it keeps its
  own structure instead of the root README story order. Its diagram is
  `profile/ecosystem-3.svg`, a single black-background SVG built with the xewe_diagram_builder
  skill. Change its file name when it changes, because GitHub's image proxy caches by URL.
- `CONTRIBUTING.md`, `SECURITY.md` and `PULL_REQUEST_TEMPLATE.md` apply to every repository in
  the organization that has no copy of its own.
- The folder name `ISSUE_TEMPLATE/` and the frontmatter keys of each template in it. The file
  names inside may change, but `?template=<file>` links name them, including the module proposal
  link in `profile/README.md`.

Other repositories link to `guidelines/*.md`, `guidelines/license-header.txt` and
`CONTRIBUTING.md` by full GitHub URL. Renaming one breaks those links, so grep the sibling
repositories before a rename and list what has to change.

There are no workflow templates here. CI is the reusable workflows in
`xewe-os-tools/.github/workflows/`, and each repository holds a short caller.
