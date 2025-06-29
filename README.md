# tauri-wdio-win-test

## Motivation

The main purpose of this repository is to try running a WebdriverIO based E2E test of a Tauri v2 app on a Windows based runner in GitHub Actions.

## Implementation description

This repository contains a minimal Tauri v2 app and an example end-to-end test for it implemented with WebdriverIO and tauri-driver mostly according to the Tauri v2 testing documentation at https://tauri.app/develop/tests/webdriver/ but also including a recent version of TypeScript for the tests.

### Issues with recent versions of WebdriverIO

At first I tried to setup the WebdriverIO config from scratch using the instructions [here](https://tauri.app/develop/tests/webdriver/example/webdriverio/#initializing-a-webdriverio-project) under the collapsed section titled "Click me if you want to see how to set a project up from scratch" but I ran into hard issues using the latest version of WebdriverIO (`@wdio/*` packages) and I could not find any way to make it work. I think the main issues was the combination of recent WebdriverIO with `tauri-driver`, but the issue could have been something else too. I had to downgrade to WebdriverIO v7 to make it work and had to figure out the typings for `wdio.config.ts` to make TypeScript happy.
