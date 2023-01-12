# summarize-text-audio
Summarizes Text from Images Using AI and send back an audio file that you can listen to.

## Technologies used:
- [GCP Cloud Vision API](https://cloud.google.com/vision)
- [Open AI](https://openai.com/)
- [GCP text-to-speech API](https://cloud.google.com/text-to-speech)
- [Twilio](https://twilio.com)
  
  Content, content, content! Are you overwhelmed by the amount of content you’re asked to read on a daily basis? Don’t you wish you could quickly summarize large chunks of text? It’d be a huge timesaver, especially for college students who read a lot of content!

This project teach you how to build an app in Python that performs text recognition on photos, summarizes that text, and then sends you a summary via SMS.

![Summarize_Text_from_Images_using_AI_and_Twilio](https://user-images.githubusercontent.com/95039067/211203135-f7f54b8f-0b36-4049-976a-d77e05292294.gif)


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

![image](https://user-images.githubusercontent.com/95039067/211203224-e0418b02-15b6-431f-882e-f313e5399504.png)


Next, enable billing for the project we just created. But don’t worry, you won’t be charged unless you exceed the Cloud Vision monthly limits

![image](https://user-images.githubusercontent.com/95039067/211203267-3317d1b7-729f-46d0-b90b-2ee5b99f226b.png)

Enable the Vision API for the project we created earlier, called summarize-text.

Next, set up authentication with a service account. Go to Create a service account, select our project (summarize-text), in the Service account name field, enter a name of summarize-text, in the Service account description field, enter a description of Service account for summarize-text. Continue and then grant the role of Project > Owner to your service account.

After creating a service account, create a service account key by clicking on the email address of service account: summarize-text. Click Keys, then Create new key. After doing this, a JSON key file will be downloaded to your computer. You’ll need to store this file in a location of your choice and then set an environment variable pointing to the path of this JSON file.

For example, on Linux or MacOS, in .zshrc:

<img width="493" alt="image" src="https://user-images.githubusercontent.com/95039067/211203592-6c18e29e-b35b-4b3e-a1d4-27e1a71c82c5.png">

For example, on Windows with PowerShell:

<img width="481" alt="image" src="https://user-images.githubusercontent.com/95039067/211203606-d5398041-8886-460a-b7c3-b46d95615932.png">

Next, install the Google Cloud CLI. Since this is different for each operating system, follow the steps outlined in Google’s gcloud CLI installation guide.

Finally, install the Python client library with the following command:

<img width="487" alt="image" src="https://user-images.githubusercontent.com/95039067/211203629-5b38138e-b029-4efa-99a7-64a491214b8b.png">

# Setup OpenAI
Assuming you already registered for an account with OpenAI, you’ll need to create an API key in your user account settings, which will allow you to authenticate your application with OpenAI. Copy this key and don’t share it with anyone!

![image](https://user-images.githubusercontent.com/95039067/211203323-40ed576c-91cc-42fe-8f76-e0a4417aa0a3.png)

We will securely store this API key in the following section.

Setup Local Environment
Create an empty project directory:

<img width="481" alt="image" src="https://user-images.githubusercontent.com/95039067/211203665-daedf90e-a0ca-4206-90ca-09a4e00ca893.png">

Then change into that directory as that’s where our code will be.

<img width="451" alt="image" src="https://user-images.githubusercontent.com/95039067/211203687-8ce8c510-0822-4d3b-b0f4-c5e917021189.png">


Create a virtual environment:

<img width="437" alt="image" src="https://user-images.githubusercontent.com/95039067/211203704-e0fd680b-6269-4047-abed-50c3f5102aff.png">


Activate our virtual environment:

<img width="414" alt="image" src="https://user-images.githubusercontent.com/95039067/211203714-ec6e24f5-f25d-4afd-938a-4180445eee24.png">


Install dependencies to our virtual environment:

<img width="457" alt="image" src="https://user-images.githubusercontent.com/95039067/211203729-ed7460eb-942a-4037-b31c-d584a6a39261.png">


Let’s create a file called .env in the project’s root directory to store our API keys in environment variables.

Within that file, we’ll create an environment variable called OPENAI_API_KEY.

(Replace PASTE_YOUR_API_KEY_HERE with the API key that you copied earlier.)

<img width="446" alt="image" src="https://user-images.githubusercontent.com/95039067/211203751-b4f846e5-f273-48de-bc04-3ed30e539e56.png">

 For example:

<img width="458" alt="image" src="https://user-images.githubusercontent.com/95039067/211203836-80473c22-95ad-471d-be67-28b2604217f3.png">

Since we’ll also be working with our Twilio account, we’ll need to modify this file even more. Log into your Twilio console, then scroll down to find your Account SID and Auth Token. Add two additional lines to the .env file, but change the values to equal your unique Account SID and Auth Token.

<img width="457" alt="image" src="https://user-images.githubusercontent.com/95039067/211203862-daaff496-09de-429a-bef7-654437c7d1bd.png">

For example:

<img width="481" alt="image" src="https://user-images.githubusercontent.com/95039067/211203897-172d4b64-8be5-4f8c-9fd2-3d519fd4328d.png">


If you’re pushing these to a Git repository, please make sure to add the .env file to your .gitignore so that these credentials are secured.

We’ll be working with local images, so in your project’s root directory, create a new directory called resources. For now, it will be an empty directory, but later this is where images will be stored.

# Cloud Vision API
Since we set it up already, you may be wondering what the Vision API is. It’s a Google API that offers powerful pre-trained machine learning models through REST. With the API, you can do things like detect faces, identify places, recognize celebrities, and much more. For this app, we will be using Optical Character Recognition (OCR) to recognize text in images.

Create a file called detect.py in the project’s root directory and copy and paste the following code into the file:

<img width="494" alt="image" src="https://user-images.githubusercontent.com/95039067/211203953-74f02104-7434-4b88-a5ae-65a2dc747b1d.png">

The detect_text function will look at a local file from your computer–in this case an image from the resources/ directory called image.jpg. Then, we will read the content from that image and use the text_detection function from the Vision API to detect text. Finally, we’ll return that text.

If you were to run the detect_text function as is, it wouldn’t work since we are reading an image called image.jpg from the resources/ directory that doesn’t currently exist. But we’ll come back to this later.

Create a new file called utilities.py in the project’s root directory and paste the following code into the file:

<img width="479" alt="image" src="https://user-images.githubusercontent.com/95039067/211203988-2fd806c8-8f20-48c9-94c5-f423dc255561.png">

The save_image function will take an image url and save it as a file called image.jpg within the resources/ directory.

OpenAI API
Now that we’ve written the code for interacting with the Cloud Vision API that allows us to perform text recognition on photos, we can use the OpenAI API to summarize that text. OpenAI is an AI company (surprise, surprise) that applies models on natural language for various tasks. You give the API a prompt, which is natural language that you input, and the AI will generate a response. For example, if you input a prompt “write a tagline for an ice cream shop” you may see a response like “we serve up smiles with every scoop!”

In the project’s root directory create a file called summarize.py and paste the following code into the file:

<img width="409" alt="image" src="https://user-images.githubusercontent.com/95039067/211204063-8935039f-78ad-4812-94f1-3aa96bf50a70.png">

The summarize_prompt function uses the OpenAI API create function to respond to a prompt that we give it (generate_prompt). The model we are specifying (text-davinci-002) is OpenAI’s most capable GPT-3 model. The max_tokens parameter sets an upper bound on how many tokens the API will return, or how long our response will be. generate_prompt will create a prompt that summarizes text in one sentence. get_text_from_image will call our previously created functions from the previous section.

Twilio SMS API
Now, we’ll create the code in our application that will all us to text our Twilio phone number and get back a response. This is called sending an Inbound SMS. Think of inbound as an inbound SMS to a Twilio phone number triggering your application. In this case, we will be sending a text to a Twilio phone number (our trigger), then having it respond by sending a reply containing a summary.

Create a new file (in the same directory) called app.py. Using Flask, a Python web framework, we will create an app that runs on a local server. Paste the following code into app.py:

<img width="385" alt="image" src="https://user-images.githubusercontent.com/95039067/211204094-f99d332e-1abf-4803-9923-39144fc81055.png">

Run the application on your local server with this command in your console (from the root directory):

Your application should be running on http://localhost:8080. Output will look similar to this:

<img width="391" alt="image" src="https://user-images.githubusercontent.com/95039067/211204169-b8864cec-4aa3-4cb3-a33e-9da50ff7095b.png">

As of now, our application is only running on a server within your computer. But we need a public-facing URL (not http://localhost) to configure a Webhook so Twilio can find it. By using a tool, called ngrok, we will “put localhost on the Internet” so we can configure our webhook.

Within the Console, enter in the ngrok URL as a Webhook when “A Message Comes In”.

![image](https://user-images.githubusercontent.com/95039067/211204213-21e6414e-9898-4629-996a-0e9e4110152d.png)

Please be aware that unless you have a paid ngrok account, each time you run the ngrok command a new URL will be generated, so be sure to make the changes within the Twilio console.

Since our application and ngrok are running, we can send a text message to our Twilio phone number and it will respond back with a summary of text!






