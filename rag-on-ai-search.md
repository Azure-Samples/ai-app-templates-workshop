
# RAG with Azure AI Search

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/azure-search-openai-demo

For this project, you must first deploy to Azure before you can run it locally, since it requires multiple Azure services.

* [Opening the project](#opening-the-project)
* [Deploying to Azure](#deploying-to-azure)
* [Running the app "locally"](#running-the-app-locally)
* [Customizing the app](#customizing-the-app)

## Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=599293758&machine=standardLinux32gb&devcontainer_path=.devcontainer%2Fdevcontainer.json&location=WestUs2)

👀 The Codespace will display the default project README when it opens. Ignore that one, and follow this guide instead.

## Deploying to Azure

1. Login with your Azure account. You'll need to use an account that has permissions to create resource groups, so a personal account may work better than a corporate account.

    ```shell
    azd auth login
    ```

    In Codespaces, this should use the device code flow where you copy and paste a code into the login window.

2. Create a new azd environment:

    ```shell
    azd env new
    ```

    This will create a folder in `.azure` to store the configuration for the deployment.

3. Set azd environment variables to use the Azure OpenAI proxy service:

    1. Run these three commands with no change:
       
        ```shell
        azd env set OPENAI_HOST azure_custom
        azd env set AZURE_OPENAI_CHATGPT_DEPLOYMENT gpt-35-turbo
        azd env set AZURE_OPENAI_EMB_DEPLOYMENT text-embedding-ada-002
        ```

    2. Replace the value before running this command:
  
       ```shell
        azd env set AZURE_OPENAI_API_KEY YOUR-KEY-HERE
        ```

    3. Replace the value before running this command:
        
        ```shell
        azd env set AZURE_OPENAI_CUSTOM_URL https://YOUR-ENDPOINT-HERE/api/v1
        ```
  
4. Deploy the app:

    ```shell
    azd up
    ```

    When you are first prompted for a location, choose a location that you haven't used before.
    When you are prompted for an OpenAI resource location, you may select *any* value as it won't actually be used.
    It will then create a new resource group with Azure App Service, AI Search, Document Intelligence, and Blob Storage,
    and deploy the app code to the App Service.

5. Once the deployment is complete, access the app at the URL provided in the terminal. Ask one of the sample questions displayed to confirm that it is working.

## Running the app "locally"

This step will run the app in a local server inside GitHub Codespaces.

1. Navigate to the `app` folder.
2. Run `./start.sh` to start the app. This will compile the frontend and start the backend.
3. Access the app by clicking `http://localhost:50505` in the VS Code terminal or from the Ports tab.
4. Ask one of the sample questions displayed to confirm that it is working. Check the citations and thought process for the answer.

## Customizing the app

Here are ideas for ways to customize this app:

1. Put your own documents in the `data/` folder and re-run `./scripts/prepdocs.py` to index them in Azure AI Search. Learn more from the [data ingestion guide](https://github.com/Azure-Samples/azure-search-openai-demo/blob/main/docs/data_ingestion.md).

2. Customize the app UI to match your company or style, by editing the frontend components. See [customization guide](https://github.com/Azure-Samples/azure-search-openai-demo/blob/main/docs/customization.md#customizing-the-ui).

3. When you've made any changes, commit them to a fork so that you can keep them. Codespaces will save your changes for a while, but they will be lost once the Codespace expires.
