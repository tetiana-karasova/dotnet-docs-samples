<walkthrough-metadata>
  <meta name="title" content="Create/Get/Update/Delete product tutorial" />
  <meta name="description" content="How to use Retail API Product Service methods" />
  <meta name="component_id" content="593554" />
  <meta name="short_id" content="true" />
</walkthrough-metadata>

# Create/Get/Update/Delete product tutorial

## Introduction

In this tutorial you will learn how to use Retail API Product Service methods, which are exposed to perform the following methods:

- CreateProduct
- GetProduct
- UpdateProduct
- DeleteProduct

You will start with creating a simple product, then call the `GetRetailProduct` method. Next you will update some product fields, and finally remove the product from the catalog.

For more information about managing catalog data, see the [Retail API documentation](https://cloud.google.com/retail/docs/manage-catalog).

<walkthrough-tutorial-duration duration="5"></walkthrough-tutorial-duration>

## Get started with Google Cloud Retail

<i>This step is required if this is the first Retail API tutorial that you run.
Otherwise, you can skip it.</i>

### Select your project and enable the Retail API

Google Cloud organizes resources into projects. This lets you
collect all related resources for a single application in one place.

If you don't have a Google Cloud project yet or you're not the owner of an existing one, you can
[create a new project](https://console.cloud.google.com/projectcreate).

After the project is created, set your PROJECT_ID to a ```project``` variable:

1. Run the following command in the Terminal:

    ```bash
    gcloud config set project <YOUR_PROJECT_ID>
    ```

1. Check that the Retail API is enabled for your project in the [Admin Console](https://console.cloud.google.com/apis/api/retail.googleapis.com/).

### Set up authentication

**Note**: Click the copy to Cloud Shell button <walkthrough-cloud-shell-icon></walkthrough-cloud-shell-icon> next to the code box to paste the command in the Cloud Shell terminal.

To run a code sample from the Cloud Shell, you need to authenticate using
the service account credentials:

1.  Log in with your user credentials.

    ```bash
    gcloud auth login
    ```

1.  Type `Y` and press **Enter**. Click the link in the Terminal. A browser window
    should appear asking you to log in using your Gmail account.

1.  Provide the Google Auth Library with access to your credentials and paste
    the code from the browser to the Terminal.

### Create service account

To access the Retail API, you must create a service account. Check that you are an owner of your Google Cloud project on the [IAM page](https://console.cloud.google.com/iam-admin/iam).

1. To create a service account, perform the following command:

    ```bash
    gcloud iam service-accounts create <SERVICE_ACCOUNT_ID>
    ```

1. Assign the needed roles to your service account:

    ```bash
    for role in {retail.editor,storage.admin,bigquery.admin};
    do gcloud projects add-iam-policy-binding crs-interactive-tutorials --member="serviceAccount:test-7@crs-interactive-tutorials.iam.gserviceaccount.com" --role="roles/${role}";
    done
    ```

1. Use the following command to print out the service account email:

    ```bash
    gcloud iam service-accounts list|grep <SERVICE_ACCOUNT_ID>
    ```

    Copy the service account email.


1.  Upload your service account key JSON file and use it to activate the service
    account:

    ```bash
    gcloud iam service-accounts keys create ~/key.json --iam-account <YOUR_SERVICE_ACCOUNT_EMAIL>
    ```

    ```bash
    gcloud auth activate-service-account --key-file ~/key.json
    ```

1.  Set the key as the GOOGLE_APPLICATION_CREDENTIALS environment variable to
    use it for sending requests to the Retail API.

    ```bash
    export GOOGLE_APPLICATION_CREDENTIALS=~/key.json
    ```

### Install Google Cloud Retail libraries

To run .NET code samples for the Retail API tutorial, you need to set up your virtual environment.

1. Next, install Google packages:

    ```bash
    dotnet add package Google.Cloud.Retail.V2

    ```

## Clone the Retail code samples

<i>This step is required if this is the first Retail API tutorial that you run.
Otherwise, you can skip it.</i>
Clone the Git repository with all the code samples to learn the Retail features and check them in action.

1. Run the following command in the Terminal:

    ```bash
    cd cloudshell_open
    git clone https://github.com/GoogleCloudPlatform/dotnet-docs-samples.git
    ```

    The code samples for each of the Retail services are stored in different directories.

1. Go to the code samples directory, your starting point to run more commands.

    ```bash
    cd retail/interactive-tutorial/RetailProduct.Samples
    ```

## Product object overview

This tutorial provides overview of the Retail API requests to manage products such as:

- `CreateProductRequest`
- `GetProductRequest`
- `UpdateProductRequest`
- `DeleteProductRequest`

Then you will run the code sample and will be able to check the Retail API responses in a Terminal output.

You can check the Product object in the [Retail documentation](https://cloud.google.com/retail/docs/reference/rpc/google.cloud.retail.v2#google.cloud.retail.v2.Product)

The required Product fields arethe following:

- `name`—a full resource name of the product, which is:

    ```none
    projects/<project_number>/locations/global/catalogs/<catalog_id>/branches/<branch_id>/products/<product_id>
    ```
- `Id`—product identifier, which is the final component of the product name.
- `Type`—the type of the product. The default value is `PRIMARY`.
- `PrimaryProductId`—a variant group identifier required for `VARIANT` products.
- `Categories[]`—names of categories that the product belongs to. This can represent different category hierarchies.
- `Title`—the product title that will be visible to a customer.

## Generate a simple product object

In this tutorial you will create a simple `PRIMARY` product presented in JSON format:

```json
{
  "name": "projects/<PROJECT_NUMBER>/locations/global/catalogs/default_catalog/branches/default_branch/products/crud_product_id",
  "id": "crud_product_id",
  "type": "PRIMARY",
  "categories": [
    "Speakers and displays"
  ],
  "brands": [
    "Google"
  ],
  "title": "Nest Mini",
  "availability": "IN_STOCK",
  "price_info": {
    "price": 30.0,
    "original_price": 35.5,
    "currency_code": "USD"
  }
}
```

Open the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailProducts.Samples/CrudProductSample.cs" regex="# generate product for update">CrudProductSample.cs</walkthrough-editor-select-regex> code sample and check this product generation with the `GenerateProduct()` method.

## The create_product request

To create a product, send a `CreateProductRequest` request to Retail API with the following required fields specified:

    - **Product**—the product object you want to create
    - `ProductId`—product ID
    - `Parent`—a branch name in a catalog where the product will be created

Check the `CreateProductRequest` request along with the Retail API call in a <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailProducts.Samples/CrudProductSample.cs" regex="# create product">`CreateProduct()`</walkthrough-editor-select-regex> method.

In the response the created Product is returned.

Further in this tutorial you will run the code sample and will be able to check the response.

## The get_product request

To build the `GetProductRequest` request, only the `name` field is required. You should send on the full resource name of the product, which is:

```
projects/<project_number>/locations/global/catalogs/<catalog_id>/branches/<branch_id>/products/<product_id>
```

You can find the `GetProductRequest` example in a <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailProducts.Samples/CrudProductSample.cs" regex="# get product">`GetProduct()`</walkthrough-editor-select-regex> method.

The Retail API returns the requested product with all product fields.

## The update_product request

To update a product you should send an `UpdateProductRequest` request to the Retail API with the following required fields specified:

 - `Product`—the product object to be updated or created (depending on the  `AllowMissing` value, the product can be created if it's missing).
 - `UpdateMask`—indicates which fields in the provided product should be updated.
 - `AllowMissing`—if the value is set to `true`, and the product is not found, a new product is created.


### Prepare data for the update request

To update each of the product fields, set the product object in the **product** request field.

Take a look at the `generate_product_for_update()` method that returns the product object with updated fields except for these fields: `name`, `id`, and `type`—these fields are immutable.

```JSON
{
  "name": "projects/<PROJECT_NUMBER>/locations/global/catalogs/default_catalog/branches/default_branch/products/<PRODUCT_ID>", #cannot be updated , should point to existent product
  "id": "<PRODUCT_ID>", #cannot be updated
  "type": "PRIMARY", #cannot be updated
  "categories": [
    "Updated Speakers and displays"
  ],
  "brands": [
    "Updated Google"
  ],
  "title": "Updated Nest Mini",
  "availability": "OUT_OF_STOCK",
  "price_info": {
    "price": 20.0,
    "original_price": 55.5,
    "currency_code": "EUR"
  }
}
```

Check the `UpdateProductRequest` example in the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailProducts.Samples/CrudProductSample.cs" regex="# update product">`UpdateProduct()`</walkthrough-editor-select-regex> method.

In the Retail API response an updated product is returned.

## The delete_product request

To build the `DeleteProductRequest` request, only the `name` field is required.

You should use the full resource name of a product, such as:

```
projects/<project_number>/locations/global/catalogs/<catalog_id>/branches/<branch_id>/products/<product_id>
```

1. Check the `DeleteProductRequest` example in the <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailProducts.Samples/CrudProductSample.cs" regex="# delete product">`DeleteProduct()`</walkthrough-editor-select-regex> method.

The DeleteProduct() method is void.

## Run the code sample and check the response

1. Run the sample in the Terminal using the command:

    ```bash
    dotnet run -- CrudProductTutorial
    ```

Running the code sample all the Retail methods will be called sequentally.
The requests and the responses will be printed out in the Terminal output.


## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to test the managing products by yourself.

<walkthrough-inline-feedback></walkthrough-inline-feedback>
