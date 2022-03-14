﻿<walkthrough-metadata>
  <meta name="title" content="Rejoin user events tutorial" />
  <meta name="description" content="Use this method if you want to to rejoin the user events in the catalog." />
  <meta name="component_id" content="593554" />
</walkthrough-metadata>

# Rejoin user events tutorial

## Introduction

The Retail API exposes methods for managing user events.

The rejoin operation joins specified events with the latest version of the product catalog.

A user event is considered unjoined if the product it's associated with isn't present in the catalog at the time that the user event is ingested. Unjoined events lack detailed product information and are not as useful to training models and serving results.

In addition to addressing unjoined events, the rejoin operation can be used to correct events that have been joined with the wrong product catalog.

<walkthrough-tutorial-duration duration="3.0"></walkthrough-tutorial-duration>

## Get started with Google Cloud Retail

This step is required if this is the first Retail API tutorial you run.
Otherwise, you can skip it.

### Select your project and enable the Retail API

Google Cloud organizes resources into projects. This lets you
collect all the related resources for a single application in one place.

If you don't have a Google Cloud project yet or you're not the owner of an existing one, you can
[create a new project](https://console.cloud.google.com/projectcreate).

After the project is created, set your PROJECT_ID to a ```project``` variable.
1. Run the following command in Terminal:
    ```bash
    gcloud config set project <YOUR_PROJECT_ID>
    ```

1. Check that the Retail API is enabled for your project in the [Admin Console](https://console.cloud.google.com/ai/retail/).

### Set up authentication

To run a code sample from the Cloud Shell, you need to be authenticated using the service account credentials.

1. Login with your user credentials.
    ```bash
    gcloud auth login
    ```

1. Type `Y` and press **Enter**. Click the link in Terminal. A browser window should appear asking you to log in using your Gmail account.

1. Provide the Google Auth Library with access to your credentials and paste the code from the browser to the Terminal.

1. Upload your service account key JSON file and use it to activate the service account:

    ```bash
    gcloud iam service-accounts keys create ~/key.json --iam-account <YOUR_SERVICE_ACCOUNT_EMAIL>
    ```

    ```bash
    gcloud auth activate-service-account --key-file  ~/key.json
    ```

1. Set key as the GOOGLE_APPLICATION_CREDENTIALS environment variable to be used for requesting the Retail API:
    ```bash
    export GOOGLE_APPLICATION_CREDENTIALS=~/key.json
    ```

**Note**: Click the copy button on the side of the code box to paste the command in the Cloud Shell terminal and run it.

### Set the PROJECT_NUMBER and PROJECT_ID environment variables

Because you are going to run the code samples in your own Google Cloud project, you should specify the **project_number** and **project_id** as environment variables. It will be used in every request to the Retail API.

1. Find the project number and project ID in the Project Info card displayed on **Home/Dashboard**.

1. Set **project_number** with the following command:
    ```bash
    export PROJECT_NUMBER=<YOUR_PROJECT_NUMBER>
    ```
1. Set **project_id** with the following command:
    ```bash
    export PROJECT_ID=<YOUR_PROJECT_ID>
    ```

### Install Google Cloud Retail libraries

1. Next, install Google packages:
    ```bash
    dotnet add package Google.Cloud.Retail.V2
    dotnet add package Google.Cloud.Storage.V1

    ```

## Clone the Retail code samples

This step is required if this is the first Retail API tutorial you run.
Otherwise, you can skip it.

Clone the Git repository with all the code samples to learn the Retail features and check them in action.

<!-- TODO(ianan): change the repository link -->
1. Run the following command in the Terminal:
    ```bash
    git clone https://github.com/GoogleCloudPlatform/dotnet-docs-samples.git
    ```

    The code samples for each of the Retail services are stored in different directories.

1. Go to the ```interactive-tutorial``` directory. This is our starting point to run more commands.
    ```bash
    cd retail/interactive-tutorial/RetailEvents.Samples
    ```

## Rejoin user events

The `RejoinUserEventsRequest` method consists of two fields:
- `Parent`—required field. The parent catalog name, such as `projects/<YOUR_PROJECT_NUMBER>/locations/global/catalogs/default_catalog`.
- `UserEventRejoinScope`—required field. The scope of user events to be rejoined with the latest product catalog. If the rejoining is intended to reduce the number of unjoined events, set the `UserEventRejoinScope` field to `UNJOINED_EVENTS`. If the rejoining is intended to correct product catalog information in joined events, set the `UserEventRejoinScope` field to `JOINED_EVENTS`. If all events need to be rejoined, set the `UserEventRejoinScope` field to `USER_EVENT_REJOIN_SCOPE_UNSPECIFIED`.

Learn more about the user events in [the Retail documentation](https://cloud.google.com/retail/docs/reference/rpc/google.cloud.retail.v2#rejoinusereventsrequest).

You can check the `RejoinUserEventsRequest` example in the `RetailEvents.Samples/RejoinUserEventSample.cs` file.

1. Check the `RejoinUserEventsRequest` request example in the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailEvents.Samples/RejoinUserEventSample.cs" regex="# get rejoin user event request">`RetailEvents.Samples/RejoinUserEventSample.cs`</walkthrough-editor-select-regex> file.

1. Run the code sample in the Terminal with the following command:
    ```bash
   dotnet run -- RejoinUserEventTutorial
    ```

The rejoin operation might take up to 24 hours. If the long-running operation is successful, then `rejoined_user_events_count`, the number of user events that were joined with the latest product catalogs, is returned.

## Error handling

Next, check the error handling by sending a rejoin request with an invalid parent.

1. Find the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailEvents.Samples/RejoinUserEventSample.cs" regex="# TO CHECK THE ERROR HANDLING TRY TO PASS INVALID CATALOG:">comment</walkthrough-editor-select-regex> and uncomment the next line.

1. Run the code sample in the Terminal with the following command:
    ```bash
    dotnet run -- RejoinUserEventTutorial
    ```

1. Check the error message:
    ```terminal
    google.api_core.exceptions.NotFound: 404 catalog_id 'invalid_catalog' not found for project. In most cases, this should be set to 'default_catalog'.
    If you just created this resource (for example, by activating your project), it might take up to 5 minutes for the resource to be activated.
    ```

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to test rejoining the user events with different combinations.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

### Do more with the Retail API

<walkthrough-tutorial-card id="retail__retail_api_v2_purge_user_events_dotnet" icon="LOGO_NET" title="Purge user events tutorial" keepPrevious=true>Try to purge user events via the Retail API</walkthrough-tutorial-card>

<walkthrough-tutorial-card id="retail_api_v2_write_user_events_dotnet" icon="LOGO_NET" title="Write user events tutorial" keepPrevious=true>
Try to write user events via the Retail API</walkthrough-tutorial-card>