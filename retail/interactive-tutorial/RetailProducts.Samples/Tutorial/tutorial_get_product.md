<walkthrough-metadata>
  <meta name="title" content="Get product tutorial" />
  <meta name="description" content="In this tutorial you will call the `GetProduct()` method and check the service response." />
  <meta name="component_id" content="593554" />
</walkthrough-metadata>

# Get product tutorial

## Get started

To fill the catalog or to update a massive number of products, we recommend using the `import_products` method. However,
sometimes you might need to make some detached changes in your product catalog.

For such cases, the Retail API provides you with the following methods:
- CreateProduct
- GetProduct
- UpdateProduct
- DeleteProduct

In this tutorial you will call the `get_product()` method and check the service response.

For information about managing catalog information, see the [Retail API documentation](https://cloud.google.com/retail/docs/manage-catalog).

<walkthrough-tutorial-duration duration="4"></walkthrough-tutorial-duration>

## Get started with Google Cloud Retail

This step is required if this is the first Retail API Tutorial you run.
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

To run .NET code samples for the Retail API tutorial, you need to set up your virtual environment.

1. Next, install Google packages:
    ```bash
    dotnet add package Google.Cloud.Retail.V2

    ```

## Clone the Retail code samples

This step is required if this is the first Retail API Tutorial you run.
Otherwise, you can skip it.

Clone the Git repository with all the code samples to learn the Retail features and check them in action.

1. Run the following command in the Terminal:
    ```bash
    git clone https://github.com/GoogleCloudPlatform/dotnet-docs-samples.git
    ```

    The code samples for each of the Retail services are stored in different directories.

1. Go to the ```interactive-tutorial``` directory. This is our starting point to run more commands.
    ```bash
    cd retail/interactive-tutorial/RetailProduct.Samples
    ```

## Get a product

To build `GetProductRequest`, only the `name` field is required. You should pass the full resource name of the product, which is:
    ```
    projects/<project_number>/locations/global/catalogs/<catalog_id>/branches/<branch_id>/products/<product_id>
    ```

1. Find the `GetProductRequest` example in a <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailProducts.Samples/GetProductSample.cs" regex="# get product request">GetProductSample.cs</walkthrough-editor-select-regex>.

1. Run this code sample in the Terminal to create a product in a catalog and get it using a prepared request.
    ```bash
    dotnet run -- GetProductTutorial
    ```

1. The Retail API returns the requested product with all product fields despite the list of retrievable fields provided in  `Product.RetrievableFields`. It defines which product fields should be displayed only in a search response.

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to test the getting products by yourself.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

### Do more with the Retail API

<walkthrough-tutorial-card id="retail__retail_api_v2_create_product_dotnet" icon="LOGO_DOTNET" title="Create product tutorial" keepPrevious=true>Try to create a product via the Retail API</walkthrough-tutorial-card>

<walkthrough-tutorial-card id="retail__retail_api_v2_update_product_dotnet" icon="LOGO_DOTNET" title="Update product tutorial" keepPrevious=true>Try to update a product via the Retail API</walkthrough-tutorial-card>

<walkthrough-tutorial-card id="retail__retail_api_v2_delete_product_dotnet" icon="LOGO_DOTNET" title="Delete product tutorial" keepPrevious=true>Try to remove a product via the Retail API</walkthrough-tutorial-card>