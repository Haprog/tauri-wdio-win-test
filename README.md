# tauri-wdio-win-test

## Motivation

The main purpose of this repository is to try running a WebdriverIO based E2E test of a Tauri v2 app on a Windows based runner in GitHub Actions.

## Implementation description

This repository contains a minimal Tauri v2 app and an example end-to-end test for it implemented with WebdriverIO and tauri-driver mostly according to the Tauri v2 testing documentation at https://tauri.app/develop/tests/webdriver/ but also including a recent version of TypeScript for the tests.

### Repository structure

The repository root contains a minimal Cargo based Tauri v2 app (does not use Tauri CLI) and an npm-based WebdriverIO testing setup.

If you already have a Tauri app for which you would like to add some E2E tests, you only need to care about the files related to the testing setup which are:

- `.github/workflows/main.yml` (WIP)
- `test/*`
- `package-lock.json`
- `package.json`
- `tsconfig.json`

### Issues with recent versions of WebdriverIO

At first I tried to setup the WebdriverIO config from scratch using the instructions [here](https://tauri.app/develop/tests/webdriver/example/webdriverio/#initializing-a-webdriverio-project) under the collapsed section titled "Click me if you want to see how to set a project up from scratch" but I ran into hard issues using the latest version of WebdriverIO (`@wdio/*` packages) and I could not find any way to make it work. I think the main issues was the combination of recent WebdriverIO with `tauri-driver`, but the issue could have been something else too. I had to downgrade to WebdriverIO v7 to make it work and had to figure out the typings for `wdio.config.ts` to make TypeScript happy.

See the linked Tauri issues at the bottom.

## Development

### Prerequisites

1. See [Tauri Prerequisites](https://tauri.app/start/prerequisites/#windows) (Rust, Node.js).

2. Install `tauri-driver` (see [Tauri WebDriver docs](https://tauri.app/develop/tests/webdriver/#system-dependencies)).

```shell
cargo install tauri-driver --locked
```

3. Make sure you have matching versions of [Edge WebView2](https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section) and [Edge WebDriver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/) installed. Recent versions of Windows should have Edge WebView2 installed but it might not be updated to the latest version automatically even if there has been a recent new release of Edge or Edge WebDriver. You can manually update to the latest WebView2 version if needed (see the link above). If the versions don't match the tests will most likely fail with weird errors.

### Testing configuration in `wdio.conf.ts`

- The `onPrepare` hook builds the Tauri application before running tests (by executing `cargo build --release`). You could remove this if testing a pre-built application.

- The `beforeSession` hook starts the `tauri-driver` by executing `.cargo/bin/tauri-driver` installed under the user's home directory.

  - This uses the `capabilities[0]["tauri:options"].application` value to determine the application binary (`.exe`) which will be executed and tested.

- The `afterSession` hook stops the `tauri-driver`.

### Tests

- `test/specs/example.e2e.ts` (Example test from Tauri testing docs)
- `test/specs/test.e2e.ts` (Example test created by `npx wdio conf` which tests an example app hosted at https://the-internet.herokuapp.com/. This test is just for reference and is currently excluded by the `exclude` section in `wdio.conf.ts`)

#### Run the tests

```shell
npm test
```

## Related links

### Documentation

- Tauri v2: [Develop / Tests](https://tauri.app/develop/tests/)
- [WebdriverIO v7](https://v7.webdriver.io/docs/gettingstarted)
- [GitHub Actions Runner Images](https://github.com/actions/runner-images)

### Issues

- https://github.com/tauri-apps/tauri/issues/10670
- https://github.com/tauri-apps/tauri/issues/9203
