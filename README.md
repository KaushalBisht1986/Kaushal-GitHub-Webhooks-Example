Print a message when a webhook delivery is received

Webhook Proxy URL: https://smee.io/WjKD3YiGIn3KfYV
To receive forwarded webhooks from smee.io, run the following command in your terminal: smee --url https://smee.io/WjKD3YiGIn3KfYV --path /webhook --port 3000
You should see output that looks like this, where WEBHOOK_PROXY_URL is your webhook proxy URL:
    Forwarding https://smee.io/WjKD3YiGIn3KfYV to http://127.0.0.1:3000/webhook
    Connected https://smee.io/WjKD3YiGIn3KfYV
Code to handle webhook deliveries is in 'helloWorld.js'. This JavaScript file handles the event types (issues and pings).
In a separate terminal window, run the following command to start a local server on your computer or codespace: node helloWorld.js
You should see output that says Server is running on port 3000.
Trigger your webhook. For example, if you created a repository webhook that is subscribed to the issues event, open an issue in your repository.
Navigate to your webhook proxy URL on smee.io. You should see an event that corresponds to the event that you triggered or redelivered. This indicates 
that GitHub successfully sent a webhook delivery to the payload URL that you specified.
In the terminal window where you ran smee --url WEBHOOK_PROXY_URL --path /webhook --port 3000, you should see something like POST http://127.0.0.1:3000/webhook - 202. 
This indicates that smee successfully forwarded your webhook to your local server.
In the terminal window where you ran node FILE_NAME, you should see a message corresponding to the event that was sent. For example, if you use the example code from 
above and you redelivered the ping event, you should see "GitHub sent the ping event".
