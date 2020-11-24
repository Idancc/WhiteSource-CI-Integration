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

3. Add a GH action to use the template:

```
name: CI step name
on:
  pull_request:
    branches: [ master ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # Checks-out your repository 
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}
        
      - name: WhiteSource CI integration
        uses: Idancc/WhiteSource-CI-Integration@v2.3.3
        env:
          WHITESOURCE_PRODUCT_NAME: ${{ secrets.WHITESOURCE_PRODUCT_NAME }}
          WHITESOURCE_PROJECT_NAME: ${{ github.event.repository.name }}
          WHITESOURCE_GH_PAT: ${{ secrets.WHITESOURCE_GH_PAT }}
          WHITESOURCE_CONFIG_REPO: ${{ secrets.WHITESOURCE_CONFIG_REPO }}
          WHITESOURCE_NPM_TOKEN: ${{ secrets.WHITESOURCE_NPM_TOKEN }}
          WHITESOURCE_API_KEY: ${{ secrets.WHITESOURCE_API_KEY }}
```
      



