# insta-post-link-extractor

**Extract links from an Instagram post.**

**Why?** Because Instagram doesn't allow you to add clickable links in the caption of a post. From a few researches, the original idea was to avoid spam, as well as let users stay within the Instagram app. However, this is a limitation for content creators who want to share **CTA links** in their posts.

**Instagram Marketers** all over the world have been trying to find ways to bypass this limitation.

One of the most common ways is to add a link in the bio, and then add a caption like **"Link in bio"**. However, this is not ideal as it requires the user to click on the profile, and then click on the link in the bio.
Users could have been enjoying other contents in the feed, but now they are distracted by the **"Link in bio"** caption as they are forced to click on the profile to find the link. Also, this must be done by the content creator.

Another way if you are using **iOS 15 or later**, is to take a screenshot of the current screen in Instagram App and use the **"Live Text"** feature to copy the link from the image. However, this is not reliable due to these two reasons:

This is a **workaround** to extract links from an Instagram post.

Comparsion of three different methods:
Method | Step by step | Live Text | This Shortcut |
---|---|---|---
Speed | 20s | 10s | 5s |
Reliability | High | Low | High |
Description | Distracting user from the original content, requires the content creator to maintain the link list. | Very distracting. Requires user to copy post link, re-open in a browser, then manual drag and highlight the link. | Not reliable | |
Sample | <img src="media/slowest-way.gif" width="240px" /> | <img src="media/live-text-way.gif" width="240px" />" | <img src="media/shortcuts-way.gif" width="240px" />

## Deployment

Open Cloudshell: <https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1>, or in an AWS Region of your choice.

```shell
git clone https://github.com/gabrielkoo/insta-post-link-extractor.git
cd insta-post-link-extractor
# Avoid `PythonPipBuilder:Validation` error as Python3.12 is not available in Cloudshell yet
sam build -x InstaPostLinkExtractorFunction
sam deploy \
    --stack-name InstaPostLinkExtractor \
    --guided
# Follow the prompts
```

Now, copy the values of `InstaPostLinkExtractorApi` and `InstaPostLinkExtractorApiUrl` from the output of the deployment:

```
CloudFormation outputs from deployed stack
-----------------------------------------------------------------------------------------------------------------
Outputs
-----------------------------------------------------------------------------------------------------------------
Key                 InstaPostLinkExtractorApiApiKeyConsoleUrl
Description         API Gateway API Key Console URL for InstaPostLinkExtractorApi
Value               https://us-east-1.console.aws.amazon.com/apigateway/main/api-keys/[API_KEY_ID_HERE]?region=us-east-1

Key                 InstaPostLinkExtractorApi
Description         API Gateway endpoint URL for Prod stage for InstaPostLinkExtractorApi
Value               https://[APIGW_ID_HERE].execute-api.us-east-1.amazonaws.com/prod/insta-post-link-extractor/
------------------------------------------------------------------------------------------------------------------

Successfully created/updated stack - insta-post-link-extractor in us-east-1
```

```shell
export ENDPOINT_URL='Paste the value of InstaPostLinkExtractorApi here'
export API_KEY='Paste the value of InstaPostLinkExtractorApiUrl here'

# Try once first!
curl $ENDPOINT_URL \
    -X POST \
    -H "x-api-key: $API_KEY" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{"url": "https://www.instagram.com/p/DB5QbNzTQxY/"}' | jq

# {
#   "links": [
#     "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
#     "https://www.youtube.com/watch?v=nsCIeklgp1M"
#   ]
# }
```

Now, install the shortcut (it was signed by Apple) <https://raw.githubusercontent.com/gabrielkoo/insta-post-link-extractor/refs/heads/main/Extract+IG+Post+Links.shortcut>.

Fill in the values of the API endpoint and the API key from the results above.

Now start saving 10 seconds of your life every time you want to extract links from an Instagram post!
