# generate-release-changelog

GitHub Action for generating a changelog of all the git commits between the new tag and the latest tags. 

Based on [ScottBrenner/generate-changelog-action](https://github.com/ScottBrenner/generate-changelog-action). 
Intended to be used with [actions/create-release](https://github.com/actions/create-release).

## Example workflow - create a release
Extends [actions/create-release: Example workflow - create a release](https://github.com/actions/create-release#example-workflow---create-a-release) to generate changelog from git commits and use it as the body for the GitHub release.

```yaml
name: Create Release

on:
  push:
    tags:
      - 'v*' # Push events to matching v*, i.e. v1.0, v20.15.10
  
jobs:
  build:
    name: Create Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Changelog
        uses: Bullrich/generate-release-changelog@master
        id: Changelog
        env:
          REPO: ${{ github.repository }}
      - name: Create Release
        id: create_release
        uses: actions/create-release@latest
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This token is provided by Actions, you do not need to create your own token
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body: |
            ${{ steps.Changelog.outputs.changelog }}
          draft: false
          prerelease: false
```

The above workflow will create a release that looks like:
![Release](release.png)

For more information, see [actions/create-release: Usage](https://github.com/actions/create-release#usage).

