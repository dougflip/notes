## Error Handling

https://softwareengineering.stackexchange.com/questions/209693/best-practices-to-create-error-codes-pattern-for-an-enterprise-project

```ts
type Error = {
  // general high level error
  // should be OK for outside user to see
  description: string;
  // identifying info that is more specific about what happened
  // can contain internal details and should be omitted in production responses
  detail: string;
  // a category code that can be safely shown in production
  // has meaning to internal dev team but not meaning externally
  errorCode: string;
  // a unique ID for this error
  // used to identify the exact instance in logs
  errorId: string;
  // used when you are wrapping an existing exception
  originalError: unknown;
};
```

## Configure NPM for CI

```sh
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
```

## Configure NPM for Artifactory

Point a particular "scope" to a different remote.
This uses an ENV var for the token which is nice from a Docker perspective.

```sh
@dougflip:registry=https://dougflip.jfrog.io/artifactory/api/npm/npm/
//dougflip.jfrog.io/artifactory/api/npm/npm/:_authToken=${RT_NPM_AUTH_TOKEN}
```

## Verify Version is Unique

```sh
#!/bin/bash

###
# Verify that the version listed in package.json has never been released.
# We do this by checking the tags in git (as oppossed to querying the registry).
###

: "${VERSION:?Required VERSION env var is not set - exiting.}"

echo "Fetching tags"
git fetch --unshallow --tags

if git tag | grep -qi ${VERSION}; then
    echo "The version, ${VERSION}, is already in use."
    echo "Here is the list of existing versions:"
    git tag
    echo ""
    echo "Please apply a new and unique version in package.json"
    exit 1
fi

echo "Success: Version $VERSION is available."
```

## Configure Git for CI

```sh
#!/bin/bash

git config user.name github-actions
git config user.email github-actions[bot]@users.noreply.github.com
```

## GitHub Action Set Vars

```sh
- name: set variables
  id: vars
  run: |
    echo "::set-output name=version::$(head -n 1 VERSION)"
    echo "::set-output name=zip_installer_amd::fred-installer-amd.tar.gz"
    echo "::set-output name=zip_installer_arm::fred-installer-arm.tar.gz"
```
