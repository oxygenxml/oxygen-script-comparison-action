# Oxygen Comparison Action
This action triggers <i>Oxygen Scripting</i> to perform a comparison between two branches on your repository. All you have to do is to include this action in your workflow and choose the branches (or commits' SHA) to compare. Find more info about workflows on https://docs.github.com/en/actions/using-workflows.

# Requirements
In order to use this action, you need to obtain an <i>Oxygen Scripting</i> license key from https://www.oxygenxml.com/xml_scripting/pricing.html (you can also request a [trial](https://www.oxygenxml.com/xml_scripting/register.html)). Add it as a secret to your repository (Settings &rarr; Secrets &rarr; Actions &rarr; New repository secret), and name it "SCRIPTING_LICENSE_KEY".
Then use it as an environment variable:
```yaml
env:
  SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
```

# Sample usage

If you don't already have a workflow defined in your repository, you can use one of the samples below.

💡 This workflow automatically runs when a pull request is opened on main branch:
```yaml
name: Run Comparison Script
on:
  pull_request_target:
    branches: [ "master" ]
    types: [opened]
  
jobs:     
  compare:
    runs-on: ubuntu-latest
    steps:
      - name: Comparison Action
        uses: FloriNNic/oxygen-scripting-comparison-action@v1.0.3
        env:
          SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
        with:
          firstBranch: master
          secondBranch: ${{ github.event.pull_request.head.sha }}
```
💡 This workflow requires manual trigger from the 'Actions' tab:
```yaml
name: Run Comparison Script
on:
  workflow_dispatch:
    inputs:
      firstBranch:
        description: 'First branch name / commit SHA'
        default: 'main'
        required: true
      secondBranch:
        description: 'Second branch name / commit SHA'
        default: 'main'
        required: true
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Oxygen Comparison Script
        uses: oxygenxml/oxygen-scripting-comparison-action@v1.0.0
        env:
          SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
        with:
          firstBranch: ${{ inputs.firstBranch }}
          secondBranch: ${{ inputs.secondBranch }}
```
# Deployment to GitHub Pages
After a successful run of the Comparison Script, a "comparisonReport.html" file, which provides details about the changes that were made, is created on the <i>gh-pages</i> branch. 
If you want this report to be published to GitHub Pages, all you have to do is go to Settings &rarr; Pages, and under <i>Build and deployment</i> section select the <i>gh-pages</i> branch instead of the <i>main</i> branch. 
The deployment workflow should automatically start and the report should be available shortly at: https://{userid}.github.io/{reponame}/comparisonReport.html.

