# WhiteSource-CI-Integration

Github action for CI integration.

How to use the action:
1. Create a repository with all your config files in the following format:
      repoName/ProductName/ProjectName/wss-generated-file-CI.config
      
      In the config file change the API key to "<API_TOKEN>" 
      
2. Define the following secrets:

* WHITESOURCE_GH_PAT:       GH private access token (enable SSO if needed, defined in the organization level)
* WHITESOURCE_CONFIG_REPO:  The path to your config files repository in the following format organization/repoName.git (defined in the organization level)
* WHITESOURCE_PRODUCT_NAME: The product name from the WS UI (define this variable in the repository level to support many products if needed)
* WHITESOURCE_API_KEY:      Your organization WS API token (defined in the organization level)
* WHITESOURCE_NPM_TOKEN:    NPM token to access private organizations if needed (defined in the organization level)
* WHITESOURCE_USER_KEY:     Your WS user key

3. Add a GH action to use the template:
a. Node projects: Change the Node version in line 10 as needed
```
name: WhiteSource CI integration
on:
  pull_request:
    branches: [ develop, release, master ]
jobs:
  WhiteSource:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [10.x]
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm ci
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}
      - name: WhiteSource CI integration
        uses: Idancc/WhiteSource-CI-Integration@v2.6
        env:
          WHITESOURCE_PRODUCT_NAME: ${{ secrets.WHITESOURCE_PRODUCT_NAME }}
          WHITESOURCE_PROJECT_NAME: ${{ github.event.repository.name }}
          WHITESOURCE_GH_PAT: ${{ secrets.WHITESOURCE_GH_PAT }}
          WHITESOURCE_CONFIG_REPO: ${{ secrets.WHITESOURCE_CONFIG_REPO }}
          WHITESOURCE_NPM_TOKEN: ${{ secrets.WHITESOURCE_NPM_TOKEN }}
          WHITESOURCE_API_KEY: ${{ secrets.WHITESOURCE_API_KEY }}
          WHITESOURCE_USER_KEY: ${{ secrets.WHITESOURCE_USER_KEY }}
      - name: policy Rejection Summary
        if: ${{ always() }}
        run: cat ./whitesource/policyRejectionSummary.json
```
b. Other:
```

name: WhiteSource CI integration
on:
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}
        
      - name: WhiteSource CI integration
        uses: Idancc/WhiteSource-CI-Integration@v2.5.1
        env:
          WHITESOURCE_PRODUCT_NAME: ${{ secrets.WHITESOURCE_PRODUCT_NAME }}
          WHITESOURCE_PROJECT_NAME: ${{ github.event.repository.name }}
          WHITESOURCE_GH_PAT: ${{ secrets.WHITESOURCE_GH_PAT }}
          WHITESOURCE_CONFIG_REPO: ${{ secrets.WHITESOURCE_CONFIG_REPO }}
          WHITESOURCE_NPM_TOKEN: ${{ secrets.WHITESOURCE_NPM_TOKEN }}
          WHITESOURCE_API_KEY: ${{ secrets.WHITESOURCE_API_KEY }}
          WHITESOURCE_USER_KEY: ${{ secrets.WHITESOURCE_USER_KEY }}
        
      - name: policy Rejection Summary
        if: ${{ always() }}
        run: cat ./whitesource/policyRejectionSummary.json
```
c. Python projects: change the Python version if needed, and install required  libraries in the 'Install dependencies' step
```
# This is a basic workflow to help you get started with Actions

name: WhiteSource CI integration

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  pull_request:
    branches: [ master ]
  push:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.6]

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Install python requierments 
      - uses: actions/checkout@v2
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v2
        with:
           python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          pip install virtualenv pipenv
          
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}

      - name: WhiteSource CI integration
        uses: Idancc/WhiteSource-CI-Integration@v2.5.1
        env:
          WHITESOURCE_PRODUCT_NAME: ${{ secrets.WHITESOURCE_PRODUCT_NAME }}
          WHITESOURCE_PROJECT_NAME: ${{ github.event.repository.name }}
          WHITESOURCE_GH_PAT: ${{ secrets.WHITESOURCE_GH_PAT }}
          WHITESOURCE_CONFIG_REPO: ${{ secrets.WHITESOURCE_CONFIG_REPO }}
          WHITESOURCE_NPM_TOKEN: ${{ secrets.WHITESOURCE_NPM_TOKEN }}
          WHITESOURCE_API_KEY: ${{ secrets.WHITESOURCE_API_KEY }}
          WHITESOURCE_USER_KEY: ${{ secrets.WHITESOURCE_USER_KEY }}
        
      - name: policy Rejection Summary
        if: ${{ always() }}
        run: cat ./whitesource/policyRejectionSummary.json
        
      - name: test 
        run: echo $SCAN_TYPE
```



