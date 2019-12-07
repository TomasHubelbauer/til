# TIL

## To-Do

### Set up a TIL scanner and digest maker

Set up a GitHub Actions workflow which uses my `github-api` library to
enumerate all my public and private repositories and look through their
recent commits (since the last run - so the last day, in `master` only
or on all branches is to be decided) and pick out the ones where the
commit description starts with TIL. Collect all the TIL entries and
make a digest, which then gets appended to the existing TIL collection
in the readme of this repository which is categorized by the date.
