**Simple web service that listens for organization events to know when a issue has been created. When an issue is created this web service prints a message.**

- When you create a webhook, you specify a URL and subscribe to event types. When an event that your webhook is subscribed to occurs, GitHub will send an HTTP request with data about the event to the URL that you specified. If your server is set up to listen for webhook deliveries at that URL, it can take action when it receives one. Thisis an example how to write code to let your server listen for and respond to webhook deliveries. You can test your code by using your computer or codespace as a local server.

- In order to test your webhook locally, you can use a webhook proxy URL to forward webhooks from GitHub to your computer or codespace. This web service uses smee.io to provide a webhook proxy URL and forward webhooks.
- If you don't already have smee-client installed, run the following command in your terminal: npm install --global smee-client
- Webhook Proxy URL: https://smee.io/WjKD3YiGIn3KfYV
- To receive forwarded webhooks from smee.io, run the following command in your terminal: smee --url https://smee.io/WjKD3YiGIn3KfYV --path /webhook --port 3000
- You should see output that looks like this:
    - Forwarding https://smee.io/WjKD3YiGIn3KfYV to http://127.0.0.1:3000/webhook
    - Connected https://smee.io/WjKD3YiGIn3KfYV
- Note that the path is /webhook and the port is 3000. You will use these values later when you write code to handle webhook deliveries.
- Keep this running while you test out your webhook. When you want to stop forwarding webhooks, enter

- Create a webhook with the following settings
    - For the URL, use your webhook proxy URL from earlier.
    - If you have an option to choose the content type, use JSON.

- Code to handle webhook deliveries is in 'helloWorld.js'. This JavaScript file handles the event types (issues and pings).
- To test your webhook, you can use your computer or codespace to act as a local server.
    - Make sure that you are forwarding webhooks.
    - In a separate terminal window, run the following command to start a local server on your computer or codespace: node helloWorld.js
    - You should see output that says Server is running on port 3000.
    - Trigger your webhook. For example, if you created a repository webhook that is subscribed to the issues event, open an issue in your repository.
    - Navigate to your webhook proxy URL on smee.io. You should see an event that corresponds to the event that you triggered or redelivered. This indicates 
that GitHub successfully sent a webhook delivery to the payload URL that you specified.
- In the terminal window where you ran smee --url https://smee.io/WjKD3YiGIn3KfYV --path /webhook --port 3000, you should see something like POST http://127.0.0.1:3000/webhook - 202. 
This indicates that smee successfully forwarded your webhook to your local server.
- In the terminal window where you ran node FILE_NAME, you should see a message corresponding to the event that was sent. For example, if you use the example code from 
above and you redelivered the ping event, you should see "GitHub sent the ping event".
- In both terminal windows, enter Ctrl+C to stop your local server and stop listening for forwarded webhooks.
