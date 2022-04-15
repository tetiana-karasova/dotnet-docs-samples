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
 
## Prediction service. Price reranking

Price reranking orders the recommended products with a similar recommendation probability by price.
The recommendation relevance is still a main reranking factor. The price reranking is not the same as sorting by price.

The price reranking can be set when creating a serving configuration in Cloud Console, 
that setting applies to all recommendations served by that configuration with no additional actions required.

You can control the price reranking of a particular recommendation in the Retail API using the PredictRequest.params field.
It overrides any configuration-level reranking settings that would otherwise apply to the recommendation.

To rerank the products on the request-level, adjust the request with one of the following values in the `Params.PriceRerankLevel` field:
   -`no-price-reranking`, 
   -`low-price-reranking`, 
   -`medium-price-reranking`, 
   -`high-price-reranking`. 

To edit the PredictRequest in the <...>, add the following field to the request and check the recommended products:

```params.PriceRerankLevel = 'low-price-reranking'```

Run the code sample:
    ```bash
    dotnet run -- PredictionWithParametersTutorial
    ```

Next, change the parameter value to `high-price-reranking`, run the code again, and check how the order of the products in the response has changed.

## Prediction service. Recommendation diversity

Diversification affects whether results returned in a single prediction response are from different categories of your product catalog or not.

Diversification can be set on the serving configuration level or per prediction request.
If a diversification is set when creating a serving configuration in Cloud Console, 
it applies by default to all recommendations served by that configuration with no additional action required.

You can control the diversity of a particular recommendation in the Retail API using the `PredictRequest.params` field. 
It overrides any configuration-level diversification settings that would otherwise apply to the recommendation.

To adjusts prediction results based on a product category, set the Params.DiversityLevel field with one of the following values:
-`no-diversity`, 
-`low-diversity`, 
-`medium-diversity`, 
-`high-diversity`, 
-`auto-diversity`.

To edit the PredictRequest in the <...>, add the following field to the request. Then, check if the recommended products belong to the same product category:

    ```Params.DiversityLevel = 'low-diversity'```

Run the code sample:
    ```bash
    dotnet run -- PredictionWithParametersTutorial
    ```
Next, change the parameter value to `high-diversity`, run the code again, and check if the products in the response belongs to different categories.

## Error handling

The error message could be received if the `params` field is set with an invalid parameter value.
Try to request predictions the following parameter:
    ```dotnet
    Params.DiversityLevel = 'invald-diversity'
    ```
Run the code sample and check the returned error message:

```
 Grpc.Core.RpcException: Status(StatusCode="InvalidArgument", Detail="Unsupported recommendationModel.diversityLevel invald-diversity")
```

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have completed the tutorial! We encourage you to experiment with getting predictions by yourself.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

### Do more with the Retail Recommendations

<walkthrough-tutorial-card tutorialid="retail__retail_api_v2_prediction_filtering_dotnet" icon="LOGO_DOTNET" title="Prediction filtering tutorial" keepPrevious=true>
Try To get a prediction filtering via the Retail API</walkthrough-tutorial-card>