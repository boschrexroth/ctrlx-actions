# Actions to build Apps for ctrlX
GitHub Actions to build Snaps for the [ctrlX Core](https://apps.boschrexroth.com/microsites/ctrlx-automation/en/portfolio/ctrlx-core/) form [Bosch Rexroth](https://www.boschrexroth.com/).

## Usage
The examples below shows how to use this action in your GitHub Workflow. 

### Build Snap
Create snap for amd64 architecture (for Virtual Control)
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
        uses: boschrexroth/ctrlx-action/build-snap@v1
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
        uses: boschrexroth/ctrlx-action/build-snap@v1
        with: 
            architecture: arm64 #required (amd64/arm64)
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: '*.snap'
```

### Build App
Create app file 
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
        uses: boschrexroth/ctrlx-action/build-app@v1
      - 
        name: Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v0.0.1
          files: binaries.zip
```

### Validate JSON schema

Validate JSON for ctrlX specific schemas
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
        uses: boschrexroth/ctrlx-action/validate-json-schema@v1
        with:
          json-schema: test-schema.json
          json-file: test.json
```
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

Copyright (c) 2021 Bosch Rexroth AG

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