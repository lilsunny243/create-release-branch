# MetaMask/action-create-release-pr

This repository can be used on its own but is better used along with: https://github.com/MetaMask/action-publish-release


Add the following Workflow File to your repository in the path `.github/workflows/create-release-pr.yml`


```
name: Create Release Pull Request

on:
    workflow_dispatch:
        inputs:
            release-version:
                description: 'A specific version to bump to.'
                required: true

jobs:
    monorepo-release-pr:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v2
              with:
                  # This is to guarantee that the most recent tag is fetched.
                  # This can be configured to a more reasonable value by consumers.
                  fetch-depth: 0
            - name: Get Node.js version
              id: nvm
              run: echo ::set-output name=NODE_VERSION::$(cat .nvmrc)
            - uses: actions/setup-node@v2
              with:
                  node-version: ${{ steps.nvm.outputs.NODE_VERSION }}
            - uses: MetaMask/action-create-release-pr@v0.0.11
              env:
                  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```
