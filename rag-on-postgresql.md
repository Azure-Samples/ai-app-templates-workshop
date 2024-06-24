
# RAG on PostgreSQL

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/rag-postgres-openai-python/

* [Opening the project](#opening-the-project)
* [Running the app "locally"](#running-the-app-locally)
* [Deploying to Azure](#deploying-to-azure)
* [Customizing the app](#customizing-the-app)

## Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new/Azure-Samples/rag-postgres-openai-python)

👀 The Codespace will display the default project README when it opens. Ignore that one, and follow this guide instead.

## Running the app "locally"

This step will run the app in a local server inside GitHub Codespaces.

1. Copy `.env.sample` to `.env` and fill in the values for `AZURE_OPENAI_ENDPOINT` and `AZURE_OPENAI_KEY`, to use the Azure OpenAI proxy service:

    ```bash
    AZURE_OPENAI_ENDPOINT=https://YOUR-ENDPOINT-HERE/api/v1
    AZURE_OPENAI_KEY=YOUR-KEY-HERE
    ```

2. Install the FastAPI backend dependencies, set up the local database, and seed it with test data:

    ```bash
    python3 -m pip install -e src
    python ./src/fastapi_app/setup_postgres_database.py
    python ./src/fastapi_app/setup_postgres_seeddata.py
    ```

2. Build the frontend:

    ```bash
    cd src/frontend
    npm install
    npm run build
    cd ..
    ```

    There must be an initial build of static assets before running the backend, since the backend serves static files from the `src/static` directory.

3. Run the FastAPI backend (with hot reloading). This should be run from the root of the project:

    ```shell
    python3 -m uvicorn fastapi_app:create_app --factory --reload
    ```

    Or you can run "Backend" in the VS Code Run & Debug menu.

    This will start the app on port 8000, and you can access it by clicking the URL in the terminal or clicking the globe icon from the Ports tab in the bottom panel.

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

## Customizing the app

Here are ideas for ways to customize this app:

1. Add an additional filter to the function calling in [query_rewriter.py](https://github.com/Azure-Samples/rag-postgres-openai-python/blob/main/src/fastapi_app/query_rewriter.py) to filter by a different column like `brand`.

2. Change the database schema for a new use case, like a movies chatbot, by modifying the schema in [postgres_models.py](https://github.com/Azure-Samples/rag-postgres-openai-python/blob/main/src/fastapi_app/postgres_models.py) and the seed data in [seed_data.json](https://github.com/Azure-Samples/rag-postgres-openai-python/blob/main/src/fastapi_app/seed_data.json).
