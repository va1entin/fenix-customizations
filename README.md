# fenix-customizations

This repo contains patches and workflows to build stable [Fenix](https://github.com/mozilla-mobile/fenix/) (Firefox for Android) releases with the [add-on collection override feature](https://blog.mozilla.org/addons/2020/09/29/expanded-extension-support-in-firefox-for-android-nightly/) otherwise only present in unstable Nightly builds.

I apply them continuously and build signed APKs [here](https://github.com/va1entin/fenix-mod).

## To make a modified release

Clone the fenix-mod repo and add upstream remote

```bash
git clone https://github.com/va1entin/fenix-mod.git
cd fenix-mod
git remote add upstream https://github.com/mozilla-mobile/fenix.git
```

Set a (stable) tag you want to modify, add customizations and push to run workflows

```bash
TAG_NAME="v101.1.0"
FORK_TAG_NAME="$TAG_NAME-mod"

git fetch upstream $TAG_NAME
git checkout $TAG_NAME
git checkout -b "$TAG_NAME-mod"
rm .github/workflows/*
cp ../fenix-customizations/*.yml .github/workflows
cp ../fenix-customizations/install-sdk.sh automation
git apply ../fenix-customizations/application_id.patch ../fenix-customizations/app_name.patch ../fenix-customizations/amo-release-102.patch
git add app automation .github
git diff --staged

git push --set-upstream origin $FORK_TAG_NAME
```
