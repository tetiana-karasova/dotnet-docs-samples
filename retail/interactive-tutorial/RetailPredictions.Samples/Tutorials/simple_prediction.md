<walkthrough-metadata>
  <meta name="title" content="Getting The Recommendations. Prediction Service" />
  <meta name="description" content="Prediction service features" />
  <meta name="component_id" content="593554" />
  <meta name="short_id" content="true" />
</walkthrough-metadata>

# Getting The Recommendations. Prediction service features

## Introduction

This is the third part of the complex tutorial "Getting The Recommendations".

Before you can request predictions from Recommendations AI, you need a trained and tuned recommendation model and one or more active serving configurations.

//TODO - add the linc to the tutorial
To learn how to prepare the data in a Retail catalog and to ingest the historical user events, go through the tutorial <...>

To create and train a recommendation model, proceed with the video tutorial: <...>

The detailed information about the recommendation service could be found in the [Retail API documentation](https://cloud.google.com/retail/docs/predict#recommendations-predict-java).

<walkthrough-tutorial-duration duration="10"></walkthrough-tutorial-duration>

## Prediction service. Simple request

1. The `PredictRequest` request has the following fields:
    - `Placement`—the ID of the Recommendations AI placement. Before you can request predictions from your model, you must create at least one placement for it.
    - `UserEvent`—context about the user, what they are looking at and what action they took to trigger the predict request.
    - `PageSize`—maximum number of results to return per page. If zero, the service will choose a reasonable default. The maximum allowed value is 100.
    - `PageToken`—the previous PredictResponse.next_page_token.
    - `Filter`—filter for restricting prediction results with a length limit of 5,000 characters. The filter can be applied to the field `tags` or can be used the `filterOutOfStockItems` flag.
    - `Params`—additional domain specific parameters for the predictions.
    - `Labels`—the labels applied to a resource. More about the labels could be found in the [documentation](https://cloud.google.com/resource-manager/docs/creating-managing-labels#requirements).
   
1. Open <walkthrough-editor-select-regex filePath="cloudshell_open/interactive-tutorial/RetailPredictions.Samples/PredictionSimpleSample.cs" regex="# get prodciction request">RetailPredictions.Samples/PredictionSimpleSample.cs</walkthrough-editor-select-regex> file and check the `PredictRequest` request.
Here is a simple prediction request with only required fields set.

1. To send this simplest PredictRequest to the Retail API, run the code sample:

    ```bash
    dotnet run -- PredictionSimpleTutorial
    ```

1. Check the response in the Terminal. The PredictionResult consists of the following fields:
   -`Results[]`-a list of recommended products. The products are ordered by relevance - from the most relevant product to the least.
   -`AttributionToken`-a unique attribution token. The attribute which is used to track the recommendation model performance.
   -`MissingIds[]`-IDs of products in the request that were missing from the inventory.
   
## Return products or scores

You can control either the response will contain the product objects or only the products scores. There are two parameters in the `PredictionRequest` to define that:
    -`ReturnProduct`-If set to true, the associated product object will be returned in the `Results.Metadata` field in the prediction response.
    -`ReturnScore`-If set to true, the prediction 'score' corresponding to each returned product will be set in the `Results.Metadata` field in the prediction response. 
                    The given 'score' indicates the probability of a product being clicked/purchased given the user's context and history.

Set the `ReturnScore` parameter, run the same code sample again and check the response:
    ```dotnet
    params.ReturnScore = true
    ```
Now the response contains only the probability score, like this:

//TODO add the example of a response

Next, add the `ReturnProduct` parameter to the `ProdictRequest`:
    ```dotnet
    params.ReturnProduct = true
    ```
Run the same code sample and check the product objects are returned.
    ```bash
    dotnet run -- PredictionSimpleTutorial
    ```
## Error handling

There are required fields in a PredictRequest:
    -`Placement`
    -`UserEvent`

If any of these fields is set with invalid value, the error message is expected. 
Try to request the Prediction service with non-existent serving config as a placement, change this field in the request in the code sample:
    ```dotnet
    Placement=f"projects/{project_id}/locations/global/catalogs/default_catalog/placements/nonexistent"
    ```
Run the code sample and check the following error message appeared:

Grpc.Core.RpcException: Status(StatusCode="NotFound", Detail="servingConfig)

The service returns an error also if the user event passed in the request is invalid.
Revert the latest changes you have made, and remove the field `UserEvent.ProductDetails.Prodcut`.

Run the code sample and check the error message

Grpc.Core.RpcException: Status(StatusCode="InvalidArgument", Detail="Field "userEvent" is a required field, but no value is found.")

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to experiment with getting predictions by yourself.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

### Do more with the Retail Recommendations

<walkthrough-tutorial-card tutorialid="retail__retail_api_v2_prediction_with_parameters_dotnet" icon="LOGO_DOTNET" title="Prediction with parameters tutorial" keepPrevious=true>
Try To get a prediction with parameters via the Retail API</walkthrough-tutorial-card>