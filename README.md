# Actions to build Apps for ctrlX

This repository contains GitHub Actions to build Apps for [ctrlX OS](https://apps.boschrexroth.com/microsites/ctrlx-automation/en/portfolio/ctrlx-os/) and [ctrlX CORE](https://apps.boschrexroth.com/microsites/ctrlx-automation/en/portfolio/ctrlx-core/) from [Bosch Rexroth](https://www.boschrexroth.com/).

You can use these actions in your repositories workflows to easily create an automated CI/CD pipeline. There are multiple actions that support you in different task (e.g. building, cross-compiling for ARM64, validation of json files, ...). 

* [Usage](#usage)
  * [Build Snap](#build-snap)
  * [Build App](#build-app)
  * [Validate JSON schema](#validate-json-schema)
* [Important direction for use](#important-direction-for-use)
  * [Areas of use and application](#areas-of-use-and-application)
  * [Unintended use](#unintended-use)
* [About](#about)
* [Licenses](#licenses)

___

## Usage

The examples below show how to use the different actions in your GitHub Workflow. 

### Build Snap

#### Example - Build Snap

Build a snap for amd64 architecture (e.g. for ctrlX CORE X7 or ctrlX COREvirtual).

```yaml
name: build ctrlX Snap (amd64)

on: push

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Build the Snap
        id: build-snap
        uses: boschrexroth/ctrlx-actions/build-snap@v1
        with: 
            architecture: amd64 # required ('amd64' or 'arm64')
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: ${{steps.build-snap.outputs.path-snap}}
```

Build a snap for arm64 architecture (e.g. for ctrlX CORE X3).

```yaml
name: build ctrlX Snap (arm64)

on: push

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Build the Snap
        id: build-snap
        uses: boschrexroth/ctrlx-actions/build-snap@v1
        with: 
            architecture: arm64 # required ('amd64' or 'arm64')
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: ${{steps.build-snap.outputs.path-snap}}
```

#### Inputs - Build Snap

| Name           | Required   | Type    | Description                                                                    |
|----------------|------------|---------|--------------------------------------------------------------------------------|
| `architecture` | `true`     | String  | Snap architecture ('amd64' or 'arm64')                                         |

#### Outputs - Build Snap

| Name           | Type    | Description                                                                                 |
|----------------|---------|---------------------------------------------------------------------------------------------|
| `path-snap`    | String  | Absolute path to the snap                                                                   |

___

### Build App

Build an App for ctrlX. A `.app` file ("App") is a package archive which combines multiple snaps for different architectures (arm64, amd64). It may also contain additional files, like digital signatures. By packaging all in a single file, it enables a simple distribution of an App for ctrlX OS.

#### Example - Build App

```yaml
name: build ctrlX Snaps and package as App

on: push

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - 
        name: Build snap for amd64
        id: amd64-build
        uses: boschrexroth/ctrlx-actions/build-snap@v1
        with: 
            architecture: amd64
      - 
        name: Build snap for arm64
        id: arm64-build
        uses: boschrexroth/ctrlx-actions/build-snap@v1
        with: 
            architecture: arm64
      - 
        name: Package App for ctrlX OS
        uses: boschrexroth/ctrlx-actions/build-app@v1
        with: 
            path-amd64-snap: ${{steps.amd64-build.outputs.path-snap}}
            path-arm64-snap: ${{steps.arm64-build.outputs.path-snap}}
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: binaries.zip
```

#### Inputs - Build App

| Name              | Required   | Type    | Description                                                                     |
|-------------------|------------|---------|---------------------------------------------------------------------------------|
| `app-name`        | `false`    | String  | Name of the .app file (default: name of repository)                             |
| `path-amd64-snap` | `false`    | String  | Path to the amd64 snap file (default: '*amd64.snap')                            |
| `path-arm64-snap` | `false`    | String  | Path to the arm64 snap file (default: '*arm64.snap')                            |

___

### Validate JSON schema

Validate JSON for ctrlX specific schemas. `.json` files are used at multiple occasions when creating an App for ctrlX. Use this action to validate those json files against corresponding json-schema which are provided by Bosch Rexroth. See: https://github.com/boschrexroth/json-schema

#### Example - Validate JSON schema

```yaml
name: validate JSON schema

on: push

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Validate JSON
        uses: boschrexroth/ctrlx-actions/validate-json-schema@v1
        with:
          json-schema: test-schema.json # path to json schema
          json-file: test-schema.json   # path to json file
```

#### Inputs - Validate JSON schema

| Name           | Required   | Type    | Description                                                                    |
|----------------|------------|---------|--------------------------------------------------------------------------------|
| `json-schema`  | `false`    | String  | Path to json schema (default: `package-manifest.v1.1.schema.json`)             |
| `json-file`    | `true`     | String  | Path to json file                                                              |
| `ajv-options`  | `false`    | String  | ajv options to parse (default: `--spec=draft2020 --strict=false --all-errors`) |

___

## Important directions for use

### Areas of use and application

The content (e.g. source code and related documents) of this repository is intended to be used for configuration, parameterization, programming or diagnostics in combination with selected Bosch Rexroth ctrlX AUTOMATION devices.
Additionally, the specifications given in the "Areas of Use and Application" for ctrlX AUTOMATION devices used with the content of this repository do also apply.

### Unintended use

Any use of the source code and related documents of this repository in applications other than those specified above or under operating conditions other than those described in the documentation and the technical specifications is considered as "unintended". Furthermore, this software must not be used in any application areas not expressly approved by Bosch Rexroth.

## About

Copyright © 2022 Bosch Rexroth AG. All rights reserved.

<https://www.boschrexroth.com>

Bosch Rexroth AG  
Bgm.-Dr.-Nebel-Str. 2  
97816 Lohr am Main  
GERMANY  

## Licenses

MIT License

Copyright (c) 2022 Bosch Rexroth AG

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
