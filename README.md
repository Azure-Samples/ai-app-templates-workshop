# AI App Templates Workshop

This guide will help you get started with the AI App Templates and the Azure OpenAI Proxy.

* [OpenAI Chat App Quickstart](#openai-chat-app-quickstart)
* [RAG on PostgreSQL](#rag-on-postgresql)

## OpenAI Chat App Quickstart

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/openai-chat-app-quickstart

### Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/openai-chat-app-quickstart)

### Local server

1. Copy `.env.sample` to `.env` and fill in the values for `AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_KEY`, and `AZURE_OPENAI_CHATGPT_DEPLOYMENT` to use the Azure OpenAI proxy service:

    ```bash
    AZURE_OPENAI_ENDPOINT=https://YOUR-ENDPOINT-HERE/api/v1
    AZURE_OPENAI_KEY=YOUR-KEY-HERE
    AZURE_OPENAI_CHATGPT_DEPLOYMENT=gpt-35-turbo
    ```

2. Run the server:

    ```bash
    python -m quart --app src.quartapp run --port 50505 --reload
    ```

    This will start the app on port 50505, and you can access it at `http://localhost:50505`.

### Deploying to Azure

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

3. Set the following azd environment variables to use the Azure OpenAI proxy service. Replace `YOUR-ENDPOINT-HERE` and `YOUR-KEY-HERE`:

    ```shell
    azd env set CREATE_AZURE_OPENAI false
    azd env set AZURE_OPENAI_CHATGPT_DEPLOYMENT gpt-35-turbo
    azd env set AZURE_OPENAI_ENDPOINT https://YOUR-ENDPOINT-HERE/api/v1
    azd env set AZURE_OPENAI_KEY YOUR-KEY-HERE
    ```

4. Deploy the app:

    ```shell
    azd up
    ```

    This will create a new resource group with an Azure Container App,
    and deploy the app code to the Azure Container App.
    Once the deployment is complete, you can access the app at the URL provided.

## RAG on PostgreSQL

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/rag-postgres-openai-python/

### Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/Azure-Samples/rag-postgres-openai-python/)

### Local server

1. Copy `.env.sample` to `.env` and fill in the values for `AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_KEY`,  `AZURE_OPENAI_CHATGPT_DEPLOYMENT`, and `AZURE_OPENAI_EMBED_DEPLOYMENT` to use the Azure OpenAI proxy service:

    ```bash
    AZURE_OPENAI_ENDPOINT=https://YOUR-ENDPOINT-HERE/api/v1
    AZURE_OPENAI_KEY=YOUR-KEY-HERE
    AZURE_OPENAI_CHAT_DEPLOYMENT=gpt-35-turbo
    AZURE_OPENAI_CHAT_MODEL=gpt-35-turbo
    AZURE_OPENAI_EMBED_DEPLOYMENT=text-embedding-ada-002
    AZURE_OPENAI_EMBED_MODEL=text-embedding-ada-002
    ```

2. Build the frontend:

    ```bash
    cd src/frontend
    npm install
    npm run build
    ```

    There must be an initial build of static assets before running the backend, since the backend serves static files from the `src/static` directory.

3. Run the FastAPI backend (with hot reloading). This should be run from the root of the project:

    ```shell
    python3 -m uvicorn fastapi_app:create_app --factory --reload
    ```

    Or you can run "Backend" in the VS Code Run & Debug menu.

    This will start the app on port 8000, and you can access it by clicking the URL in the terminal or clicking the globe icon from the Ports tab in the bottom panel.

### Deploying to Azure

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

3. Set the following azd environment variables to use the Azure OpenAI proxy service:

    ```shell
    azd env set DEPLOY_AZURE_OPENAI false
    azd env set AZURE_OPENAI_CHAT_DEPLOYMENT gpt-35-turbo
    azd env set AZURE_OPENAI_EMBED_DEPLOYMENT text-embedding-ada-002
    azd env set AZURE_OPENAI_ENDPOINT https://YOUR-ENDPOINT-HERE/api/v1
    azd env set AZURE_OPENAI_KEY YOUR-KEY-HERE
    ```

4. Deploy the app:

    ```shell
    azd up
    ```

    When you are prompted for an openAI resource location, you may select any value as it won't actually be used.
    This will create a new resource group with an Azure Container App and Azure PostgreSQL Flexible Server,
    and deploy the app code to the Azure Container App.
    Once the deployment is complete, you can access the app at the URL provided.
