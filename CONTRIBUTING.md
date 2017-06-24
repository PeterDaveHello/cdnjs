# Contribute to CDNJS

* Read the [sparseCheckout documentation](/documents/sparseCheckout.md) before you clone the CDNJS repo locally. It is not recommended to clone the whole repo as is due to its large size. Using sparse-checkout also works around issues on case-insensitive file systems (HFS+ on macOS).
* All libraries on CDNJS must meet our [popularity criteria](#popularity-requirements-for-new-libraries).

## Table of Contents

* [Issues](#issues)
    * [General Conventions for Issues](#general-conventions-for-issues)
    * [Popularity Requirements for New Libraries](#popularity-requirements-for-new-libraries)
* [Pull Requests](#pull-requests)
    * [Sparse Checkout and Shallow Clone](#sparse-checkout-and-shallow-clone)
    * [General Conventions for Pull Requests](#general-conventions-for-pull-requests)
    * [Update an Existing Library](#update-an-existing-library)
    * [Add a New Library With Assets](#add-a-new-library-with-assets)
    * [Add a New Library by a Single package.json file](#add-a-new-library-by-a-single-packagejson-file)
    * [Pre-flight Checks](#pre-flight-checks)
    * [Miscellaneous Information](#miscellaneous-information)
### Issues

#### General Conventions for Issues

* [Search](https://github.com/cdnjs/cdnjs/issues/) for possible duplicates before creating a new issue.
* If you're the author/maintainer of a library, prepend `[author]` to the title when creating issues about that library. Such issues will be processed with higher priority than others.
* Tag related [people](https://github.com/blog/821-mention-somebody-they-re-notified), [issues](https://github.com/blog/957-introducing-issue-mentions) and commits while creating a new issue to make collaboration easier.

#### Popularity Requirements for New Libraries

A library must satisfy the following criteria to be eligible for CDNJS:
* 100 stars on GitHub **or** 500 downloads in the last month on npm
* Should not be a personal hobby project
* Must have at least one official and publicly accessible repository
* Should be released under an [open source license](https://opensource.org/licenses)

### Pull Requests

#### Sparse Checkout and Shallow Clone

There are several reasons why we recommend using a combination of sparse checkout and shallow clone to manage your local copy of the CDNJS repo:

* Cloning the whole repository to make a minor change or add a new library is not worthwhile.
* The CDNJS repo on GitHub weighs around 2.7 GB and a local extracted copy would take up 70 GB of your disk space.
* In case of case-insensitive file systems such as Windows and macOS, using sparse-checkout is a simple workaround to avoid the case issue (discussion here: [#3650](https://github.com/cdnjs/cdnjs/issues/3650)).

#### General Conventions for Pull Requests

1. CDNJS only hosts library with a production/stable release. Exceptions can be made if your pre-production version (alpha/beta/RC) is [popular](#popularity-requirements-for-new-libraries) and you'd like for it to be hosted on CDNJS.
2. Rebase your CDNJS fork with upstream prior to every time you create a branch for a PR.
3. Always edit on a branch. Never, ever make edits to the `master` branch. Always make a new branch before you edit files. We follow the [GitHub Flow](https://guides.github.com/introduction/flow/index.html) workflow.

4. Don't make multiple unrelated changes in a single commit. Only strongly related changes must be included in a single commit.
5. Every commit should be meaningful. For example, don't create a new commit to fix a typo you made in your previous commit.
6. If minified files are not already available in the library you are trying to add, minify CSS and JavaScript files using [UglifyJS](https://www.npmjs.com/package/uglify-js "UglifyJS") or [web-minify-helper](https://github.com/PeterDaveHello/web-minify-helper "web-minify-helper"). Append `.min` to minified files (for example, `jquery.min.js`).
* Filenames should not consist of a version number and must be lowercase. `useful.min.js` is OKAY, but `useful-2.0.1.min.js` is NOT.

7. All titles of pull requests which add a new library must start with `[new]`.
8. If you're the author/maintainer of a library, prepend `[author]` to the title when creating a pull request for that library. Such PRs will be processed with higher priority than others.
9. Fill out all applicable fields in the PR description. This is essential to make review easier and faster.
10. Once you have created a PR, our CI will run automated tests (generally takes few minutes). If the tests fail, take a look at the error(s) and fix them. You can always ask for help. Pull requests which fail our tests will not be merged.
11. You might be asked to modify your commit(s) by one of our maintainers or the CI. Use `git commit --amend` or `git rebase` and force push the changes using `--force` to update your PR.

#### Update an Existing Library

1. Follow already existing file and directory structure. If you are fixing an incorrect structure, mention this in the commit description or pull request.
2. Ensure that you update the `package.json` file to reflect the new version number.
3. Ensure that `filename` in the `package.json` file is correct. File names might change with a new version.
4. Add [auto-update config](/documents/autoupdate.md) for the library if it doesn't already exist.

#### Add a New Library With Assets

* All libraries are stores in the `ajax/libs` directory. Each library has it's own directory in `ajax/libs` and each version of the library has it's own subdirectory. For example, jQuery v1.1.2 will be located at `ajax/libs/jquery/1.1.2/`.
* [`package.json`](https://www.npmjs.org/doc/package.json.html) file
    * The root of each library (for example, `ajax/libs/jquery`) must have a the `package.json` file. We store all library-related meta data in this file.
    * Use information from the official `package.json` file if the library has one.
    * `package.json` follows 2 space indents.
    * Use [JSONLint](http://jsonlint.com/) to validate your `package.json` structure.
* The library's directory name (located at `ajax/libs`) must be same as the `package.json` `name` field.
* All libraries must have [auto-update](/documents/autoupdate.md) set up in the `package.json` file. This means we don't have to manually add new releases. You'll however need to add files for the latest version manually when adding a new library.
* Run `npm test` locally and make sure all [tests pass](/README.md#run-npm-test-to-check-all-is-well) (learn how to [install test dependencies](/README.md#install-npm-test-dependencies)).

You can also add a new library without manually adding all assets using the method below.

#### Add a New Library by a Single package.json file

This method of adding a new library using a single `package.json` file is recommended for beginners. You can do this directly from GitHub and don't need to use the command line. Before following the steps below, remember to follow our [conventions for branches, commits and pull requests](#).

* [Fork the CDNJS repo](https://github.com/cdnjs/cdnjs/fork) to your GitHub account.
* [Create a new branch](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/).
* Navigate to the `ajax/libs` directory.
* Click on "Create new file".
* For the file name, enter `LIBRARY_NAME/package.json`. LIBRARY_NAME will be the name of the directory in `ajax/libs`. For example, `jquery/package.json`.
* Compose a valid `package.json`file following the conventions listed above. Use minified versions for `filename` in `package.json` even if they don't exist. Our maintainers will minify the library when hosting it. For example, `jquery.min.js`.
* Make a new commit and submit the pull request.

#### Pre-Flight Checks

* Have you complied with all our conventions?
* Have you followed the library's directory structure?
* Does a valid and accurate `package.json` file exist for the library?
* Have you minified CSS and JavaScript assets?
* Did all tests pass when you ran `npm test`?

#### Miscellaneous Information

* We recommend working on a Unix-like environment (for example, GNU/Linux or BSD distributions except macOS) due to how git works.
* If this document didn't answer a question of yours, refer to [CONTRIBUTING-WIP.md](https://github.com/cdnjs/cdnjs/blob/master/CONTRIBUTING-WIP.md) (might be outdated). In case of any conflicts, follow CONTRIBUTING.md. Also, open an issue and report this if you can.
* New contribution documentation is in the works and will be released soon.
* Add auto-update configuration for all libraries if it doesn't exist. This is irrespective of whether you are updating or adding the library.
* Pro tip: Use `git diff` before creating to commit to see the changes you have made. You can also use `git log -p` or `git show sha1hash` to differentiate.
* Pull requests may be taken away if there is no response for 7 days.
* Always rebase your branch with the latest `master` after updating your PR.

Thank you so much for your contribution. We hope we can make CDNJS the best CDN service worldwide.