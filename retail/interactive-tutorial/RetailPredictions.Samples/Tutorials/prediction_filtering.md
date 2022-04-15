<walkthrough-metadata>
  <meta name="title" content="Getting The Recommendations. Prediction Service" />
  <meta name="description" content="Prediction service features" />
  <meta name="component_id" content="593554" />
  <meta name="short_id" content="true" />
</walkthrough-metadata>

# Getting the recommendations. Prediction service features

## Introduction

This is the third part of the complex Getting the Recommendation tutorial.

To request predictions from Recommendations AI, you need a trained and tuned recommendation model and one or more active serving configurations.

//TODO - add the linc to the tutorial
To learn how to prepare the data in a Retail catalog and to ingest the historical user events, go through the tutorial <...>

To create and train a recommendation model, proceed with the video tutorial: <...>


The detailed information about the recommendation service could be found in the [Retail API documentation](https://cloud.google.com/retail/docs/predict#recommendations-predict-java).

<walkthrough-tutorial-duration duration="10"></walkthrough-tutorial-duration>

## Prediction service. Filter the recommendations

Filter the recommendations returned by Recommendations AI by using the `filter` field in the `PredictRequest`.

The filter field accepts two forms of filter specification:
    - filterOutOfStockItems    
    - tag expressions

If you combine these two types of filters, only items that satisfy all specified filter expressions are returned.

## Filter out of stock products

The `filterOutOfStockItems` flag filters out any products with `OUT_OF_STOCK` the availability.

Add the field `Filter` to a `PredictRequest` in the code sample, and set the `filterOutOfStockItems` flag:
```dotnet
filter: "filterOutOfStockItems"
```

Run the code and check the list of recommended products in the response:
    ```bash
    dotnet run -- PredictionFilteringTutorial
    ```
The prediction response does not contain products with the `OUT_OF_STOCK` availability status.

Next, remove the filter, run the code gain and compare the results. 
Now, all the products are returned, including the out of stock items.

## Filter by price

The recommended products can be requested and filtered by a price. 
The price range could be set using the comparison operators `>, <, <=, >=`.
Set the following filters values, run the code sample, and check the products in the response:

```filter: "price>=20.0"```
```filter: "price<49.99"```
```filter: "price>=35.0 price<50.0"```

## Filter with tag expressions 

To apply the filtering by tag expressions, the products in a catalog should have the field `tags`.

Here's an example of a simple tag expression you can apply in this tutorial:
```dotnet
filter: "tag=\"promotional\""
```
Run the code sample and check the response containing only products with the following tag:
    ```bash
    dotnet run -- PredictionFilteringTutorial
    ```

When filtering by several tags, only products that satisfy **all** specified filter expressions are returned.

To check how it works, add one more tag to the filter and run the code sample again:
```dotnet
filter: "tag=\"promotional\" tag=\"season sale\""
```

**Note:** "Recently viewed" models don't support tag filtering at the moment.

## Use boolean operators in tag expressions

Tag expressions can contain the boolean operators `OR` or `NOT`. In such a case, the expressions must be enclosed in parentheses.
* A dash (-) symbol is equival to the `NOT` operator.

Set the following filter expression:
```dotnet
filter: "tag=(\"promotional\" OR \"premium\") tag=(-\"season sale\") filterOutOfStockItems"
```
Check the response. The Prediction service returns only items that are in stock, have either the `premium` or the `promotional` tag (or both), and do not have the `season sale` tag.

## Strict filtering

If your filter blocks all prediction results, the API will return generic (unfiltered) popular products. 

Set the filter expression which definitely results in an empty product set:
```dotnet
filter: "tag=\"promotional\" tag=(-\"promotional\")"
```
Set the `strictFiltering` parameter to false:
```dotnet
Params.StrictFiltering = False
```
Run the code sample and check that prediction response contains some (popular) recommended products.

If you want to receive only results that strictly match the filters, set `Params.StrictFiltering` to `true` in the `PredictRequest` to receive empty results instead:
```dotnet
Params.StrictFiltering = True
```
Run the code sample again. Now, the prediction response now is empty.

**Note:** the API will never return items with storageStatus of "EXPIRED" or "DELETED" regardless of filter choices.

## Error handling

The error message could be received if the filtering expression is written with the syntax error.
Try to set the following filtering expression to the `Fiter` field:
```dotnet
filter: "tags=\"promotional\", \"premium\""
```
Run the code sample and check the returned error message:

```
 Grpc.Core.RpcException: Status(StatusCode="InvalidArgument", Detail="Invalid filter 'tags="promotional", "premium"')``

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to experiment with getting predictions by yourself.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

### Do more with the Retail Recommendations

<walkthrough-tutorial-card tutorialid="retail__retail_api_v2_simple_prediction" icon="LOGO_DOTNET" title="Simple prediction tutorial" keepPrevious=true>
Try To get a simple prediction via the Retail API</walkthrough-tutorial-card>