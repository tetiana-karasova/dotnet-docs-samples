﻿<walkthrough-metadata>
  <meta name="title" content="Purge user events tutorial" />
  <meta name="description" content="Use this method if you want to to purge the user events from the catalog." />
  <meta name="component_id" content="593554" />
</walkthrough-metadata>

# Purge user events tutorial

## Introduction

The Retail API exposes methods for managing user events.
If you want to purge the user events from the catalog you can use the `PurgeUserEvents` method.

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

Clone the Git repository with all the code samples to learn about the Retail features and see them in action.

1. Run the following command in the Terminal:
    ```bash
    git clone https://github.com/GoogleCloudPlatform/dotnet-docs-samples.git
    ```

    The code samples for each of the Retail services are stored in different directories.

1. Go to the ```interactive-tutorial``` directory. This is our starting point to run more commands.
    ```bash
    cd retail/interactive-tutorial/RetailEvents.Samples
    ```

## Purge user events

To send the `PurgeUserEventsRequest` request you need to specify the following fields:
- `Parent`—required field. The parent catalog name, such as `projects/<YOUR_PROJECT_NUMBER>/locations/global/catalogs/default_catalog`.
- `Filter`—required field. The filter string to specify the events to be deleted. An empty string filter is not allowed.
  The eligible fields for filtering are:
  - EventType: UserEvent.event_type string.
  - EventTime: in ISO 8601 "zulu" format.
  - VisitorId: specifying this deletes all events associated with a visitor.
  - UserId: specifying this deletes all events associated with a user.
- `Force`—if force is set to `false`, the method returns the expected purge count without deleting any user events; if set to `true`, the user events are purged from the catalog.

1. Open the`PurgeUserEventsRequest` example in the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailEvents.Samples/PurgeUserEventSample.cs" regex="# get purge user event request">`RetailEvents.Samples/PurgeUserEventSample.cs`</walkthrough-editor-select-regex> file.

1. Check that the filter field is set with the value:
    ```
    PurgeUserEventsRequest.Filter = 'VisitorId="test_visitor_id"'
    ```

1. Run the code sample in the Terminal with the following command:
    ```bash
    dotnet run -- PurgeUserEventTutorial
    ```

The long-running operation starts. You can check the operation name in the Terminal output.

The purge operation might take up to 24 hours. If the long-running operation is successful, then `PurgedEventsCount` is returned in the `google.longrunning.Operations.response` field.

## Error handling

Next, let's check the error handling. Send a request with an invalid filter.

1. Find the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailEvents.Samples/PurgeUserEventSample.cs" regex="# TO CHECK ERROR HANDLING SET INVALID FILTER HERE:">comment</walkthrough-editor-select-regex> and set the filter field with an invalid value:
    ```
    PurgeUserEventsRequest.Filter = 'Invalid="123abc"'
    ```

1. Run the code sample in the Terminal with the following command:
    ```bash
    dotnet run -- PurgeUserEventTutorial
    ```

1. Check the error message:
    ```terminal
    google.api_core.exceptions.InvalidArgument: 400 Invalid filter 'invalid="123abc"'. '=' can not be specified with 'invalid' Valid filter examples:
    eventTime>"2012-04-23T18:25:43.511Z" eventTime<"2012-04-23T18:25:43.511Z" eventType=search visitorId="someVisitorId"
    ```

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to test purging the user events using different filters.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

### Do more with the Retail API

<walkthrough-tutorial-card id="retail__retail_api_v2_rejoin_user_events_dotnet" icon="LOGO_DOTNET" title="Rejoin user events tutorial" keepPrevious=true>Try to rejoin user events via the Retail API</walkthrough-tutorial-card>

<walkthrough-tutorial-card id="retail__retail_api_v2_write_user_events_dotnet" icon="LOGO_DOTNET" title="Write user events tutorial" keepPrevious=true>
Try to write user events via the Retail API</walkthrough-tutorial-card>