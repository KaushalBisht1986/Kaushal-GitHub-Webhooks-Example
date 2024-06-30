**Simple web service that listens for organization events to know when a repository has been created. When the repository is created this web service automates the protection of the main branch. It also notifies you with an @mention in an issue within the repository that outlines the protections that were added.**

- When you create a webhook, you specify a URL and subscribe to event types. When an event that your webhook is subscribed to occurs (like repository creation), GitHub will send an HTTP request with data about the event to the URL that you specified. If your server is set up to listen for webhook deliveries at that URL, it can take action when it receives one. This is an example how to write code to let your server listen for and respond to webhook deliveries. You can test your code by using your computer or codespace as a local server.
- Install the following:
  - Python 3: https://www.python.org/downloads/
      - pip install -r requirements.txt        
  - Flask: https://flask.palletsprojects.com/en/1.1.x/installation/#installation
  - Ngrok: https://ngrok.com/download
- Code to handle webhook deliveries is in 'app.py'. This file handles the event types (repository and issue creation).
- In app.py file make the following changes:
  - Change user value to your 'GitHub username' (like KaushalBisht1986)
  - Change cred to your Fine-grained personal access tokens. Process to create personal access token: https://docs.github.com/en/enterprise-server@3.9/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens. Select the scopes you'd like to grant this token.
- In app.py file, start the local web service via command: flask run --host=0.0.0.0
- Start the forwarding service via command: ngrok http 5000
    - You will see a Forwarding url like this: https://abc.ngrok-free.app. After you create a webhook in later steps this will forward the webhook from GitHub to your local host on port 5000
- Create a webhook at organization level using steps mentioned here: https://docs.github.com/en/webhooks/using-webhooks/creating-webhooks
    - For the URL, use your Forwarding URL from earlier.
    - Choose the content type, use JSON.
- **Test the Code: ** To test your webhook, you can use your computer as a local server.
    - Make sure that you are forwarding webhooks.
    - Ensure that the local service is running
    - Trigger your webhook i.e., create a new repository and select 'Add a ReadMe file'. This will create a new repository with main as default branch 
    - Navigate to your webhook proxy URL in command prompt where ngrok is running. You should see an event that corresponds to the event that you triggered (Example. POST / 200 OK). This indicates that GitHub successfully sent a webhook delivery to the payload URL that you specified and  forwarded your webhook to your local server.
    - In the terminal window where you ran local web service, you should see a message like "POST / HTTP/1.1" 200, which means POST request is successfully sent
    - Go the the repository that you just created on GitHub UI and you will notice a branch protection rule has been automatically added. You will also notice that Issue has been created mentioning 'A new branch protection was added to the main branch.'  
- In both terminal windows, enter Ctrl+C to stop your local server and stop listening for forwarded webhooks.

**Simple web service that listens for organization events to know when an issue has been created. When an issue has been created this web service will send a message saying issue has been created.**
 
- In order to test your webhook locally, you can use a webhook proxy URL to forward webhooks from GitHub to your computer or codespace. This web service uses smee.io to provide a webhook proxy URL and forward webhooks.
- Get a webhook proxy URL
    - In your browser, navigate to https://smee.io/
    - Click Start a new channel
    - Copy the full URL under "Webhook Proxy URL". You will use this URL in the following setup steps.
- Forward Webhooks
    - If you don't already have smee-client installed, run the following command in your terminal: npm install --global smee-client
    - To receive forwarded webhooks from smee.io, run the following command in your terminal: smee --url WEBHOOK_PROXY_URL --path /webhook --port 3000
- You should see output that looks like this:
    - Forwarding WEBHOOK_PROXY_URL to http://127.0.0.1:3000/webhook
    - Connected WEBHOOK_PROXY_URL
- Note that the path is /webhook and the port is 3000. You will use these values later when you write code to handle webhook deliveries.
- Keep this running while you test out your webhook. When you want to stop forwarding webhooks, enter Ctrl+C

- Create a webhook with the following settings
    - For the URL, use your webhook proxy URL from earlier.
    - If you have an option to choose the content type, use JSON.

- Code to handle webhook deliveries is in 'helloWorld.js'. This JavaScript file handles the event types (issues and pings).
- **Test the Code: ** To test your webhook, you can use your computer or codespace to act as a local server.
    - Make sure that you are forwarding webhooks.
    - In a separate terminal window, run the following command to start a local server on your computer or codespace: node helloWorld.js
    - You should see output that says Server is running on port 3000.
    - Trigger your webhook. For example, if you created a repository webhook that is subscribed to the issues event, open an issue in your repository.
    - Navigate to your webhook proxy URL on smee.io. You should see an event that corresponds to the event that you triggered or redelivered. This indicates 
that GitHub successfully sent a webhook delivery to the payload URL that you specified.
- In the terminal window where you ran smee --url WEBHOOK_PROXY_URL --path /webhook --port 3000, you should see something like POST http://127.0.0.1:3000/webhook - 202. 
This indicates that smee successfully forwarded your webhook to your local server.
- In the terminal window where you ran node FILE_NAME, you should see a message corresponding to the event that was sent. For example, if you use the example code from 
above and you redelivered the ping event, you should see "GitHub sent the ping event".
- In both terminal windows, enter Ctrl+C to stop your local server and stop listening for forwarded webhooks.
