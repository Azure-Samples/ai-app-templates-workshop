
# OpenAI Chat App Quickstart

This guide is specifically for workshop attendees that are using the Azure OpenAI proxy service
with https://github.com/Azure-Samples/openai-chat-app-quickstart

* [Opening the project](#opening-the-project)
* [Running the app "locally"](#running-the-app-locally)
* [Deploying to Azure](#deploying-to-azure)
* [Customizing the app](#customizing-the-app)

## Opening the project

Open the project in GitHub Codespaces by clicking the button below:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/openai-chat-app-quickstart)

👀 The Codespace will display the default project README when it opens. Ignore that one, and follow this guide instead.

## Running the app "locally"

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

## Deploying to Azure

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

## Customizing the app

Here are ideas for ways to customize this app:

1. Modify the system prompt to target the chat for a particular use case, like customer support, life coach, or tutor. You can change the prompt in the `chat_handler` function in [chat.py](https://github.com/Azure-Samples/openai-chat-app-quickstart/blob/main/src/quartapp/chat.py).

2. Modify the UI to match your company or style, by editing the HTML template [index.html](https://github.com/Azure-Samples/openai-chat-app-quickstart/blob/main/src/quartapp/templates/index.html).