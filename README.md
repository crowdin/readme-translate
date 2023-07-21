# Readme Translate [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?url=https%3A%2F%2Fgithub.com%2Fcrowdin%2Freadme-translate&text=A%20GitHub%20Action%20to%20automate%20translation%20of%20your%20README.md%20files%20via%20Crowdin)&nbsp;[![GitHub Repo stars](https://img.shields.io/github/stars/crowdin/readme-translate?style=social&cacheSeconds=1800)](https://github.com/crowdin/readme-translate/stargazers)

A GitHub Action to automate the translation of your README.md files via Crowdin ✨

## What does this action do?

- Upload your `README.md` file to Crowdin
- Download the translations from Crowdin to the specified place
- Manage the languages switcher in your `README.md` file

## Usage

Set up a workflow in *.github/workflows/readme-translate.yml* (or add a job to your existing workflows).

Read the [Configuring a workflow](https://help.github.com/en/articles/configuring-a-workflow) article for more details on how to create and set up custom workflows.

```yaml
# .github/workflows/readme-translate.yml
name: Readme Translate

on:
  # When you push to the `main` branch
  push:
    branches: [ main ]
  # And optionally, once every 12 hours
  schedule:
    - cron: '0 */12 * * *' # https://crontab.guru/#0_*/12_*_*_*
  # To manually run this workflow
  workflow_dispatch:

jobs:
  readme-translate:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v3

    - name: Generate Crowdin Contributors table
      uses: crowdin/readme-translate@v0.1.0
      env:
        CROWDIN_PROJECT_ID: ${{ secrets.CROWDIN_PROJECT_ID }}
        CROWDIN_PERSONAL_TOKEN: ${{ secrets.CROWDIN_PERSONAL_TOKEN }}
        CROWDIN_ORGANIZATION: ${{ secrets.CROWDIN_ORGANIZATION }} # Optional. Only for Crowdin Enterprise
```

### Creating a PR

To create a PR with the updated table, we recommend usage of the [Create Pull Request](https://github.com/peter-evans/create-pull-request) Action:

```yaml
# .github/workflows/readme-translate.yml
name: Readme Translate

on:
  # When you push to the `main` branch
  push:
    branches: [ main ]
  # And optionally, once every 12 hours
  schedule:
    - cron: '0 */12 * * *' # https://crontab.guru/#0_*/12_*_*_*
  # To manually run this workflow
  workflow_dispatch:

jobs:
  crowdin-contributors:

    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v3

    - name: Generate Crowdin Contributors table
      uses: crowdin/readme-translate@v0.1.0
      env:
        CROWDIN_PROJECT_ID: ${{ secrets.CROWDIN_PROJECT_ID }}
        CROWDIN_PERSONAL_TOKEN: ${{ secrets.CROWDIN_PERSONAL_TOKEN }}
        CROWDIN_ORGANIZATION: ${{ secrets.CROWDIN_ORGANIZATION }} # Optional. Only for Crowdin Enterprise

    - name: Create Pull Request
      uses: peter-evans/create-pull-request@v4
      with:
        title: New Readme Translations by Crowdin
        body: By [readme-translate](https://github.com/crowdin/readme-translate) GitHub action
        commit-message: New Readme Translations
        committer: Crowdin Bot <support+bot@crowdin.com>
        branch: docs/readme-translations
```

### Automatic Pre-Translation

[//]: # (TODO)

### Languages Switcher

The Readme Translate Action will automatically add a language switcher to your `README.md` file.

To enable it, add the following placeholders to your `README.md` file:

- `<!-- README-TRANSLATE-LANGUAGES-START -->`
- `<!-- README-TRANSLATE-LANGUAGES-END -->`

## Options

| Option                 | Default value                         | Description                                            |
|------------------------|---------------------------------------|--------------------------------------------------------|
| `file`                 | `README.md`                           | The Readme file name                                   |
| `destination`          | `./`                                  | A directory where the localized readmes will be placed |
| `languages`            | All Crowdin project languages         | A list of languages to translate the Readme            |

## Contributing

If you want to contribute please read the [Contributing](/CONTRIBUTING.md) guidelines.

## Author

- [Andrii Bodnar](https://github.com/andrii-bodnar/)

## License

<pre>
The Readme Translate Action is licensed under the MIT License.
See the LICENSE file distributed with this work for additional
information regarding copyright ownership.

Except as contained in the LICENSE file, the name(s) of the above copyright
holders shall not be used in advertising or otherwise to promote the sale,
use or other dealings in this Software without prior written authorization.
</pre>
