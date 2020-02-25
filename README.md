# Scope: Getting Started
A starter iOS project instrumented with [Scope](https://scope.undefinedlabs.com) through [Scope for Swift GitHub Action](https://github.com/marketplace/actions/scope-for-swift).

This starter project is based on:
- [Xcode](https://developer.apple.com/xcode/)

## Run Scope for Swift action to run your tests

The project doesn't have any reference to Scope in its code or dependencies. The github Action configures Scope and run your tests without changes to your code/project. 

It uses carthage to link with Alamofire for networking and show networking automatic instrumentation.

All the work is done by adding the `Scope for Swift` step in the GitHub workflow

```yaml
name: Scope Testing

on: [push]

jobs:
  scope:
    runs-on: macOS-latest
    
    steps:
      - name: Check if SCOPE_DSN is set
        run: if [ "${{secrets.SCOPE_DSN}}" = "" ]; then exit 1; fi
      - name: Checkout
        uses: actions/checkout@v1
      - name: Build dependencies
        run: carthage update --platform iOS
      - name: Scope for Swift
        uses: undefinedlabs/scope-for-swift-action@v1
        with:
          dsn: ${{ secrets.SCOPE_DSN }}
          platform: ios
          codePath: true
        env:
          SCOPE_INSTRUMENTATION_HTTP_PAYLOADS: YES
```

## Running Scope on GitHub Actions

1. Click on `Use this template` button and create the repository in your namespace.
2. Access to [app.scope.dev](https://app.scope.dev). 
3. Add/Modify your namespace to include your new repository.
4. Get the API Key for your new repository.
5. Go to your repository on [GitHub](https://github.com)
6. Go to `Settings` -> `Secrets`.
7. Add your DSN secret.
    - Name: `SCOPE_DSN`
    - Value: `<<your DSN>>`
8. Click on `Actions` button and access to the workflow.
9. Click on `Re-run checks`.

Once GitHub Workflow has finished, you can check the test executions report on [app.scope.dev](https://app.scope.dev)

