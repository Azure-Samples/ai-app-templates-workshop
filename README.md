# AI App Templates Workshop

This guide will help you get started with the AI App Templates and the Azure OpenAI Proxy.

* [OpenAI Chat App Quickstart](#openai-chat-app-quickstart)
* [RAG on PostgreSQL](#rag-on-postgresql)
* [RAG with Azure AI Search](#rag-with-azure-ai-search)

## OpenAI Chat App Quickstart

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/openai-chat-app-quickstart

### Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/openai-chat-app-quickstart)

👀 The Codespace will display the default project README when it opens. Ignore that one, and follow this guide instead.

### Running the app "locally"

This step will run the app in a local server inside GitHub Codespaces.

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

3. Type any question into the chat and make sure you get a successful response back from the model.

### Deploying to Azure

1. Either quit the server or open a new terminal for this step.

2. Login with your Azure account. You'll need to use an account that has permissions to create resource groups, so a personal account may work better than a corporate account.

    ```shell
    azd auth login
    ```

    In Codespaces, this should use the device code flow where you copy and paste a code into the login window.

3. Create a new azd environment:

    ```shell
    azd env new
    ```

    This will create a folder in `.azure` to store the configuration for the deployment.

4. Set the following azd environment variables to use the Azure OpenAI proxy service. Replace `YOUR-ENDPOINT-HERE` and `YOUR-KEY-HERE`:

    ```shell
    azd env set CREATE_AZURE_OPENAI false
    azd env set AZURE_OPENAI_CHATGPT_DEPLOYMENT gpt-35-turbo
    azd env set AZURE_OPENAI_ENDPOINT https://YOUR-ENDPOINT-HERE/api/v1
    azd env set AZURE_OPENAI_KEY YOUR-KEY-HERE
    ```

5. Deploy the app:

    ```shell
    azd up
    ```

    When prompted for a location, select a location that you haven't used before.
    This will create a new resource group with an Azure Container App,
    and deploy the app code to the Azure Container App.
    Once the deployment is complete, you can access the app at the URL provided.

### Customizing the app

Here are ideas for ways to customize this app:

1. Modify the system prompt to target the chat for a particular use case, like customer support, life coach, or tutor. You can change the prompt in the `chat_handler` function in [chat.py](https://github.com/Azure-Samples/openai-chat-app-quickstart/blob/main/src/quartapp/chat.py).

2. Modify the UI to match your company or style, by editing the HTML template [index.html](https://github.com/Azure-Samples/openai-chat-app-quickstart/blob/main/src/quartapp/templates/index.html).

## RAG on PostgreSQL

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/rag-postgres-openai-python/

### Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/Azure-Samples/rag-postgres-openai-python/)

👀 The Codespace will display the default project README when it opens. Ignore that one, and follow this guide instead.

### Running the app "locally"

This step will run the app in a local server inside GitHub Codespaces.

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

### Customizing the app

Here are ideas for ways to customize this app:

1. Add an additional filter to the function calling in [query_rewriter.py](https://github.com/Azure-Samples/rag-postgres-openai-python/blob/main/src/fastapi_app/query_rewriter.py) to filter by a different column like `brand`.

2. Change the database schema for a new use case, like a movies chatbot, by modifying the schema in [postgres_models.py](https://github.com/Azure-Samples/rag-postgres-openai-python/blob/main/src/fastapi_app/postgres_models.py) and the seed data in [seed_data.json](https://github.com/Azure-Samples/rag-postgres-openai-python/blob/main/src/fastapi_app/seed_data.json).

## RAG with Azure AI Search

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/azure-search-openai-demo

For this project, you must first deploy to Azure before you can run it locally, since it requires multiple Azure services.

### Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/Azure-Samples/azure-search-openai-demo/)

👀 The Codespace will display the default project README when it opens. Ignore that one, and follow this guide instead.

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
    azd env set OPENAI_HOST azure_custom
    azd env set AZURE_OPENAI_CUSTOM_URL https://YOUR-ENDPOINT-HERE/api/v1
    azd env set AZURE_OPENAI_API_KEY YOUR-KEY-HERE
    azd env set AZURE_OPENAI_CHATGPT_DEPLOYMENT gpt-35-turbo
    azd env set AZURE_OPENAI_EMB_DEPLOYMENT text-embedding-ada-002
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

### Running the app "locally"

This step will run the app in a local server inside GitHub Codespaces.

1. Navigate to the `app` folder.
2. Run `./start.sh` to start the app. This will compile the frontend and start the backend.
3. Access the app by clicking `http://localhost:50505` in the VS Code terminal or from the Ports tab.
4. Ask one of the sample questions displayed to confirm that it is working. Check the citations and thought process for the answer.

### Customizing the app

Here are ideas for ways to customize this app:

1. Put your own documents in the `data/` folder and re-run `./scripts/prepdocs.py` to index them in Azure AI Search. Learn more from the [data ingestion guide](https://github.com/Azure-Samples/azure-search-openai-demo/blob/main/docs/data_ingestion.md).

2. Customize the app UI to match your company or style, by editing the frontend components. See [customization guide](https://github.com/Azure-Samples/azure-search-openai-demo/blob/main/docs/customization.md#customizing-the-ui).

3. When you've made any changes, commit them to a fork so that you can keep them. Codespaces will save your changes for a while, but they will be lost once the Codespace expires.
