# peach-version-checker

[![GitHub logo](/assets/github_logo.png "peach-version-checker GitHub repository")](https://github.com/peachcloud/peach-version-checker) ![Version badge](https://img.shields.io/badge/version-0.1.0-<COLOR>.svg)

A simple commandline tool which helps to ensure version consistency across software and documentation.

`peach-version-checker` compares version numbers in crate READMEs, manifests and developer documentation for each microservice and program in the PeachCloud ecosystem. Each program is given a passing or failing grade in the generated report. Project maintainers can then act accordingly if any version inconsistencies are found.

## Usage

```bash
git clone git@github.com:peachcloud/peach-version-checker.git
cd peach-version-checker
cargo build --release
./release/target/peach-version-checker
```

## Example Output

```bash
[ peach-buttons  ]
Dev-docs: 0.1.3
Manifest: 0.1.3
Readme  : 0.1.3
PASS
[ peach-oled  ]
Dev-docs: 0.1.0
Manifest: 0.1.3
Readme  : 0.1.3
FAIL
[ peach-probe  ]
Dev-docs: No version number found
Manifest: 0.1.1
Readme  : 0.1.1
FAIL
```

## Licensing

AGPL-3.0
