Maintaining React-Bootstrap

This document is for people working on React-Bootstrap. It describes common tasks such as triaging or merging pull requests.

If you are interested in contributing to React-Bootstrap, you should check out the Contributing Guide.
Triaging Issues

New issues pop up every day. We need to identify urgent issues (such as nobody can use a component, or install React-Bootstrap), close and link duplicates, answer questions, etc. Please alert the gitter/react-bootstrap chat room of the urgent issues. We are using HuBoard which is a kanban style board to track and prioritize issues.

Some issues are opened that are just too vague to do anything about. If after attempting to get feedback from issue authors fails after 7 days, then close the issue. Please inform the issue author that they may re-open if they are able to present the requested information.
Merging a pull request

Please, make sure:

    Travis build is green
    At least one collaborator (other than you) approves the PR
        Commenting "LGTM" (Looks good to me) or something of similar sorts is sufficient.
        If it's a simple docs change or a typo fix, feel free to skip this step.
    Commits follow the convention prescribed in the Contributing Guide.
        This is very important, because the release process uses this to generate the changelog.
        Commits are squashed. Each change is a single commit.
            e.g. if the PR contains two changes such as [fixed] Some Bug and then [fixed] Some other unrelated bug it should be two separate changes.
            e.g. if the first commit is [fixed] Some bug and the second commit is [fixed] failing tests for previous commit, you should squash them into a single commit
        It's alright to ask the author of the pull request to fix any of the above.

Becoming a maintainer

If you are interested in becoming a react-bootstrap maintainer, start by reviewing issues and pull requests. Answer questions for those in need of troubleshooting. Join us in the gitter/react-bootstrap chat room. Once we see you helping, either we will reach out and ask you if you want to join or you can ask one of the organization owners to add you. We will try our best to be proactive in reaching out to those that are already helping out.

GitHub by default does not publicly state that you are a member of the organization. Please feel free to change that setting for yourself so others will know who's helping out. That can be configured on the organization list page.

Being a maintainer is not an obligation. You can help when you have time and be less active when you don't. If you get a new job and get busy, that's alright.
Releases

Releases should include documentation, git tag, bower package preparation and finally the actual npm module publish. We have all of this automated by running npm run release. PLEASE DO NOT RUN npm publish BY ITSELF. The release-script will do that. We want to prevent issues like #325 and #218 from ever happening again. In order to run the release-script you will need permission to publish the package to npm. Those with this permission are in the publishers team

Note: The publishers team does exist. If you see 404 that means you just have no permissions to publish.

Example usages of the release-script:

$ npm run release patch // without "--run" it will run in "dry run" mode
$ npm run release patch -- --run
$ npm run release minor -- --run
$ npm run release major -- --run
$ npm run release minor -- --preid beta --run  Use both bump and preid for first prerelease
$ npm run release -- --preid beta --run        For follow on prereleases of the next version just use this

Note additional -- double-dash. It is important.

Or if you have this line

export PATH="./node_modules/.bin:$PATH"

in your shell config, then you can run it just as:

$ release patch // without "--run" it will run in "dry run" mode
$ release patch --run
$ release minor --preid beta --run
$ release --preid beta --run

Note that the above commands will bump the semver version programmatically so you don't need to. Please be mindful to ensure that semver guidelines are followed. If it is discovered that we have pushed a release in violation of semver, than a patch release reverting the offending change should be pushed as soon as possible to correct the error. The offending change can then be re-applied and released with the proper version bump.
Release Candidates

In an effort to reduce the frequency with which we introduce breaking changes we should do our best to first push deprecation warnings in a Minor release. Also, Pull Requests with breaking changes should be submitted against the vX-rc branch, where X is the next Major version. Which we will in turn release as an alpha release of the next Major version. When we are ready to release the next Major version bump we will merge the vX-rc branch into the master branch and cut a beta release. Once bugs have been addressed with the beta release then we will release the Major version bump.
Live releasing the documentation

The documentation release script does a similar job to the release script except that it doesn't publish to npm. It will auto tag the current branch with a pre "docs" tag, and will push to documentation repository.

For a given tag (lets say 0.22.1) the first docs tag would be 0.22.1-docs.0. In order to tags to be incremental and in order to include all the previous docs changes, make sure that if a docs tags exists for the current release, that you start from that tag.

To live patch the documentation in between release follow these steps

    Find the latest documentation release.

    Check the latest release tag (lets say v0.22.1).
    Look for a docs-release tag for that version ex: v0.22.1-docs.X
    If one exists, check-it-out. If not checkout the latest release tag.
    Note: Checkout the tag and not master directly because some live documentation changes on master that could related to new components or updates for the upcoming release

    Create a new branch from there (for example git checkout -b docs/v0.22.1)
    Cherry-pick the commits you want to include in the live update git cherry-pick <commit-ish>...
    Use the

$ npm run release -- --only-docs --run
// or
$ release --only-docs --run

to push and tag to the documentation repository.

Note: The branch name you checkout to cherry-picked the commit is not enforced. Though keeping similar names ex: docs/<version> helps finding the branch easily.
Check everything is OK before releasing

Release tools are run in "dry run" mode by default. It prevents danger steps (git push, npm publish etc) from accidental running.

You can use it

    to learn how releasing tools are working.
    to ensure there are no side issues before you release anything.

$ npm run release -- --only-docs
$ npm run release major
$ npm run release minor -- --preid beta
// or
$ release --only-docs
$ release major
$ release minor --preid beta
