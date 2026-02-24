# gcp-dicomweb-proxy

## Deploy

Deploy command:

```bash
GCP_DICOMWEB_PROXY_FN_NAME=proxy
gcloud functions deploy $GCP_DICOMWEB_PROXY_FN_NAME \
  --runtime nodejs20 \
  --trigger-http \
  --allow-unauthenticated \
  --entry-point proxy \
  --no-gen2
```

After deploying, run the following if you want it to be accessible to all users (you will need `cloudfunctions.functions.setIamPolicy` IAM permission to perform this operation):

```bash
GCP_DICOMWEB_PROXY_FN_NAME=proxy
gcloud functions add-iam-policy-binding $GCP_DICOMWEB_PROXY_FN_NAME \
  --region=us-central1 \
  --member=allUsers \
  --role=roles/cloudfunctions.invoker
```

Conversation that helped fix the code to make it deploy: https://www.perplexity.ai/search/i-am-trying-to-deploy-a-google-BjRoJupjQ2eup440PQOEAQ

Not so helpful conversation with Gemini, which however includes instructions on how to configure AppEngine SA to permit access to GHC DICOM store: https://g.co/gemini/share/46ab5254e2c8

## Permissions

Service account used by the function is listed under details of the function in https://console.cloud.google.com/functions.

## Testing

To confirm proxy is working (once deployment is successful): get URL under the "Trigger" tab in the function details (at the URL above) and test with curl:

```bash
curl <TRIGGER_URL>/studies
```
