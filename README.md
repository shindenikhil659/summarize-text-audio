# summarize-text-audio
Summarizes Text from Images Using AI and send back an audio file that you can listen to.

## Technologies used:
- [GCP Cloud Vision API](https://cloud.google.com/vision)
- [Open AI](https://openai.com/)
- [GCP text-to-speech API](https://cloud.google.com/text-to-speech)
- [Twilio](https://twilio.com)
  
  Content, content, content! Are you overwhelmed by the amount of content you’re asked to read on a daily basis? Don’t you wish you could quickly summarize large chunks of text? It’d be a huge timesaver, especially for college students who read a lot of content!

In this blog post, I will teach you how to build an app in Python that performs text recognition on photos, summarizes that text, and then sends you a summary via SMS.

https://assets.cdn.prod.twilio.com/original_images/Summarize_Text_from_Images_using_AI_and_Twilio.gif

Here’s a typical use case: you see a large wall of text that you don’t want to read, so you pull out your phone to take a picture of that text, then you receive a SMS with a nice summarization. Boom, time saved!

Prerequisites

Before getting started, it’s important to have the following before moving on:

Python 3.7 or higher installed on your machine.

A Twilio account. If you haven’t yet, sign up for a free Twilio trial.

A Google Cloud account, get started for free.

An OpenAI account, sign up for a free account.

ngrok installed on your machine. ngrok is a useful tool for connecting your local server to a public URL. You can sign up for a free account and learn how to install ngrok.

1.Setup Google Cloud Vision: Set up our Google Cloud account and enable the Vision API

2.Setup OpenAI: Set up our OpenAI account

3.Setup Local Environment: Set up our local development environment

4.Cloud Vision API: Using ML, detect words from images using the Google Cloud Vision API

5.OpenAI API: Using AI, generate a summary of text from the OpenAI API

6.Twilio SMS API: Send a text message (containing the summary) when the application is triggered

# Setup Google Cloud Vision
To use the Google Cloud Vision API, we need to set it up by following the quickstart guide. This process does take some time, but don’t get discouraged. Just follow the quickstart guide step-by-step, or continue along here (if you don’t want to tab out).

Assuming you already have a Google Cloud account, you’ll need to create a new project within Google Cloud. Give it a Project name of summarize-text and click Create.

https://assets.cdn.prod.twilio.com/images/h7CBzo1qzu_8lp--Hfs_V5bR28XJq81Ervc0cUIjvMhCjz.width-800.png

Next, enable billing for the project we just created. But don’t worry, you won’t be charged unless you exceed the Cloud Vision monthly limits

https://assets.cdn.prod.twilio.com/images/9-3ubtnZCFE1-uCuGra3YHFqwtXcozVpnWuUvbuSqx6kz9.width-800.png

Next, set up authentication with a service account. Go to Create a service account, select our project (summarize-text), in the Service account name field, enter a name of summarize-text, in the Service account description field, enter a description of Service account for summarize-text. Continue and then grant the role of Project > Owner to your service account.

After creating a service account, create a service account key by clicking on the email address of service account: summarize-text. Click Keys, then Create new key. After doing this, a JSON key file will be downloaded to your computer. You’ll need to store this file in a location of your choice and then set an environment variable pointing to the path of this JSON file.

For example, on Linux or MacOS, in .zshrc:

export GOOGLE_APPLICATION_CREDENTIALS="/home/user/Downloads/service-account-file.json"
For example, on Windows with PowerShell:

$env:GOOGLE_APPLICATION_CREDENTIALS="C:\Users\username\Downloads\service-account-file.json"
Next, install the Google Cloud CLI. Since this is different for each operating system, follow the steps outlined in Google’s gcloud CLI installation guide.

Finally, install the Python client library with the following command:

pip install --upgrade google-cloud-vision

# Setup OpenAI
Assuming you already registered for an account with OpenAI, you’ll need to create an API key in your user account settings, which will allow you to authenticate your application with OpenAI. Copy this key and don’t share it with anyone!
