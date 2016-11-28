# CockroachDB Dependencies
This is a snapshot of cockroachdb's dependencies, intended to be checked out, as a submodule, at `./vendor`.

## Updating Dependencies
This snapshot was built and is managed using `glide`.

The [docs](https://github.com/Masterminds/glide) have detailed instructions, but in brief:
* run `glide` in the parent of this directory (i.e. your `cockroachdb/cockroach` checkout)
* add new dependencies with `glide get -v -s github.com/my/dependency`
* update dependencies to their latest version with `glide up`

## Working with Submodules
Since dependendies are stored in their own repository, and `cockroachdb/cockroach` depends on it,
changing a dependency requires pushing two things, in order: first the changes to the `vendored`
repository, then the reference to those changes to the main repository.

After adding or updating dependencies (and running all tests), switch into the `vendor` submodule
and commit changes (or use `git -C`). The commit must be available on `github.com/cockroachdb/vendored`
*before* submitting a pull request to `cockroachdb/cockroach` that references it.
* Organization members can push new refs directly.
  * Likely want to `git remote set-url origin git@github.com:cockroachdb/vendored.git`.
  * If not pushing to `master`, be sure to tag the ref so that it will not be GC'ed.
* Non-members should fork the `vendored` repo, add their fork as a second remote, and submit a pull
request.
  * After the pull request is merged, fetch and checkout the new `origin/master`, and commit *that* as the
  new ref in `cockroachdb/cockroach`.

## Repository Name
We only want this directory used by builds when it is explicitly checked out *and managed* as a
submodule at `./vendor`.

If a go build fails to find a dependency in `./vendor`, it will continue searching anything named
"vendor" in parent directories. Thus this repository is _not_ named "vendor", to minimize the risk
of it ending up somewhere in `GOPATH` with the name `vendor` (e.g. if it is manually cloned), where
it could end up being unintentionally used by builds and causing confusion.
