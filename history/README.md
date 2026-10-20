# history

The record of how this repository's history was extracted from the monorepo,
[The Test Cabinet](https://github.com/TheClockwyrks/TheTestCabinet), where the
crate here lived until the extraction. Nothing builds or tests against these
files; they are kept so the provenance outlives the scratch clone the
extraction ran in.

## The extraction

The history below the scaffold commit is the monorepo's `nightly` at
`e5097a6c058749b05ac6502fc601a0b2a64eee1b`, filtered to the paths in
[`paths.txt`](paths.txt) by git filter-repo 2.47.0:

```sh
git clone -q --no-local --single-branch --branch nightly --no-tags <the-test-cabinet> x
cd x
uvx --from git-filter-repo==2.47.0 git-filter-repo --force \
  --paths-from-file ../paths.txt --prune-empty always --preserve-commit-hashes
```

Of the 6,917 commits reachable from that commit, 12 touched a path in the list
and were kept (2 of them merges); the rest were pruned. No tag was carried. The
tip tree of `crates/draw/` hashes the same as the monorepo's at that commit
(`11a0ca2bcf3ebea97eba5aa9b77919c5cb9627c5`).

The path is kept where it was in the monorepo, so the filter needed no rename.

## The files

- [`paths.txt`](paths.txt) is the list handed to `--paths-from-file`
  (sha256 `89a1b37e792c40d930cc64cbf20f4bced9c32f5d080790d2fd2d57ba86698137`).
  It is generated, not hand-written: the moved directory (`crates/draw/`), and
  every path a tracked file under it had before a rename or copy, collected by
  `git log --follow -M -C --name-status` over each of the 20 files. No file
  under it was ever renamed or copied from elsewhere, so the list is that one
  directory.
- [`commit-map`](commit-map) is filter-repo's map from each monorepo commit (left)
  to the commit it became here (right), all zeros for a pruned one.
  `--preserve-commit-hashes` left the commit ids quoted in the messages as they
  were, so an id in a message below the scaffold commit names a monorepo commit,
  and this map translates it.
