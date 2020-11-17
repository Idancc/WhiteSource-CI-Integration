# WhiteSource-CI-Integration

Github action for CI integration.

How to use the action:
1. Create a repository with all your config files in the following format:
      repoName/ProductName/ProjectName/wss-generated-file-CI.config
      
      In the config file change the API key to "<API_TOKEN>" 
      
2. Define the following secrets:

WHITESOURCE_GH_PAT: GH private access token (enable SSO if needed, defined in the organization level)
WHITESOURCE_CONFIG_REPO: The path to your config files repository in the following format organization/repoName.git (defined in the organization level)
WHITESOURCE_PRODUCT_NAME: The product name from the WS UI (define this variable in the repository level to support many products if needed)
WHITESOURCE_API_KEY: Your organization WS API token (defined in the organization level)
WHITESOURCE_NPM_TOKEN: NPM token to access private organizations if needed (defined in the organization level)
      



