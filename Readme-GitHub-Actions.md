\# Automated CV Building with GitHub Actions



This guide explains how to leverage GitHub Actions for continuous integration and automated PDF generation for your LaTeX CV.



\## Advantages of GitHub Actions



\- \*\*Full Automation\*\*: Your CV is built automatically on every push

\- \*\*Version Control\*\*: Each change is tracked and archived

\- \*\*PDF Releases\*\*: Automatically generate PDF releases with version numbers

\- \*\*No Local Setup\*\*: No need for LaTeX or Docker on your local machine

\- \*\*CI/CD Integration\*\*: Part of a professional development workflow



\## How It Works



When you push changes to the `main` or `master` branch, GitHub Actions:



1\. Sets up a LaTeX environment in the cloud

2\. Compiles your CV into a PDF

3\. Creates a release branch containing the PDF

4\. Generates a GitHub Release with the PDF attached



\## Setup Process



The repository already includes the necessary workflow configuration at `.github/workflows/build-cv.yml`.



\### Understanding the Workflow File



The workflow file contains the following key components:



```yml

name: Build LaTeX CV



on:

&#x20; push:

&#x20;   branches:

&#x20;     - main

&#x20;     - master



jobs:

&#x20; build-and-release:

&#x20;   runs-on: ubuntu-latest

&#x20;   permissions:

&#x20;     contents: write

&#x20;   steps:

&#x20;     - name: Checkout repository

&#x20;       uses: actions/checkout@v3

&#x20;       with:

&#x20;         fetch-depth: 0



&#x20;     - name: Set up LaTeX

&#x20;       uses: xu-cheng/latex-action@v2

&#x20;       with:

&#x20;         root\_file: main.tex



&#x20;     - name: Prepare PDF for release branch

&#x20;       run: |

&#x20;         mkdir -p /tmp/cv\_release

&#x20;         cp main.pdf /tmp/cv\_release/main.pdf



&#x20;     - name: Create release branch with only PDF

&#x20;       run: |

&#x20;         git config --local user.email "action@github.com"

&#x20;         git config --local user.name "GitHub Action"

&#x20;         git checkout --orphan release

&#x20;         # Remove all files and folders except .git

&#x20;         find . -mindepth 1 -maxdepth 1 ! -name '.git' ! -name '.' -exec rm -rf {} +

&#x20;         cp /tmp/cv\_release/main.pdf .

&#x20;         git add main.pdf

&#x20;         git commit -m "Update CV PDF"

&#x20;         git push -f origin release



&#x20;     - name: Create GitHub Release

&#x20;       id: create\_release

&#x20;       uses: actions/create-release@v1

&#x20;       env:

&#x20;         GITHUB\_TOKEN: ${{ secrets.GITHUB\_TOKEN }}

&#x20;       with:

&#x20;         tag\_name: v${{ github.run\_number }}

&#x20;         release\_name: Release v${{ github.run\_number }}

&#x20;         body: "Automated CV PDF build."

&#x20;         draft: false

&#x20;         prerelease: false



&#x20;     - name: Upload Release Asset

&#x20;       uses: actions/upload-release-asset@v1

&#x20;       env:

&#x20;         GITHUB\_TOKEN: ${{ secrets.GITHUB\_TOKEN }}

&#x20;       with:

&#x20;         upload\_url: ${{ steps.create\_release.outputs.upload\_url }}

&#x20;         asset\_path: ./main.pdf

&#x20;         asset\_name: main.pdf

&#x20;         asset\_content\_type: application/pdf 

```



\## Troubleshooting

1\. Go to the "Actions" tab in your repository

2\. Look for the latest workflow run

3\. Click on it to see detailed logs

4\. If there are errors, expand the failing step for details



