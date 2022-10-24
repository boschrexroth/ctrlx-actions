# Actions to build Apps for ctrlX
GitHub Actions to build Apps for the [ctrlX Core](https://apps.boschrexroth.com/microsites/ctrlx-automation/en/portfolio/ctrlx-core/) from [Bosch Rexroth](https://www.boschrexroth.com/).

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
The examples below shows how to use this action in your GitHub Workflow. 

### Build Snap
Create snap for amd64 architecture (for Virtual Control)

#### Example - Build Snap
```yaml
name: build ctrlX snap

on: push

jobs:
  ctrlX:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Build ctrlX snap
        uses: boschrexroth/ctrlx-actions/build-snap@v1
        with: 
            architecture: amd64 #required (amd64/arm64)
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: '*.snap'
```

Create snap for arm64 architecture (for physical ctrlX)
```yaml
name: build ctrlX snap

on: push

jobs:
  ctrlX:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Build ctrlX snap
        uses: boschrexroth/ctrlx-actions/build-snap@v1
        with: 
            architecture: arm64 #required (amd64/arm64)
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: '*.snap'
```

#### Inputs - Build Snap
| Name           | Required   | Type    | Description                                                                    |
|----------------|------------|---------|--------------------------------------------------------------------------------|
| `architecture` | `true`     | String  | Snap architecture (example: amd64/arm64)                                       |

___

### Build App
Create app file 

#### Example - Build App
```yaml
name: build ctrlX app

on: push

jobs:
  ctrlX:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Build ctrlX snap
        uses: boschrexroth/ctrlx-actions/build-app@v1
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: binaries.zip
```

#### Inputs - Build App
| Name           | Required   | Type    | Description                                                                    |
|----------------|------------|---------|--------------------------------------------------------------------------------|
| `app-name`     | `false`    | String  | Name of the .app file (default: name of repository)                            |

___

### Validate JSON schema
Validate JSON for ctrlX specific schemas

#### Example - Validate JSON schema
```yaml
name: validate JSON schema

on: push

jobs:
  ctrlX:
    runs-on: ubuntu-latest

    steps:
      - 
        name: Checkout
        uses: actions/checkout@v3
      - 
        name: Validate JSON
        uses: boschrexroth/ctrlx-actions/validate-json-schema@v1
        with:
          json-schema: test-schema.json #path to json schema
          json-file: test-schema.json #path to json file
```

#### Inputs - Validate JSON schema
| Name           | Required   | Type    | Description                                                                    |
|----------------|------------|---------|--------------------------------------------------------------------------------|
| `json-schema`  | `true`     | String  | Directory to json schema                                                       |
| `json-file`    | `true`     | String  | Directory to json file                                                         |
| `ajv-options`  | `false`    | String  | ajv options to parse (default: --spec=draft2020 --strict=false --all-errors)   |

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