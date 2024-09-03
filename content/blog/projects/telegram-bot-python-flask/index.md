---
title: Build a Telegram bot using Python and the Telegram API
date: "2024-08-29T22:40:32.169Z"
description: How to create an interractive Telegram bot using Python and the Telegram API.
---

To interact directly with the Telegram API without using a dispatcher or any library abstraction (such as `python-telegram-bot`), you'll need to make HTTP requests to the Telegram Bot API endpoints. Below is an example of how you can build a Telegram bot using Flask and direct API calls.

### Prerequisites:
- **Flask** for handling the webhook.
- **Requests** for making HTTP calls to the Telegram API.
  
First, install the required dependencies:

```bash
pip install Flask requests
```

### Step 1: Create a Basic Flask Application
This Flask app will receive webhook updates from Telegram and respond by making direct API calls to Telegram.

```python
import os
import requests
from flask import Flask, request, jsonify

app = Flask(__name__)

# Set your bot's token here (replace with your actual token)
TELEGRAM_BOT_TOKEN = os.getenv('TELEGRAM_BOT_TOKEN')  # You can set this directly as a string if preferred

# The base URL for Telegram API requests
TELEGRAM_API_URL = f"https://api.telegram.org/bot{TELEGRAM_BOT_TOKEN}"

# Function to send a message to a Telegram user
def send_message(chat_id, text):
    url = f"{TELEGRAM_API_URL}/sendMessage"
    payload = {
        'chat_id': chat_id,
        'text': text
    }
    response = requests.post(url, json=payload)
    return response.json()

# Webhook endpoint to receive updates from Telegram
@app.route('/webhook', methods=['POST'])
def webhook():
    # Get the incoming update from Telegram
    update = request.get_json()

    # Extract the message and chat_id from the update
    if 'message' in update:
        chat_id = update['message']['chat']['id']
        text = update['message'].get('text', '')

        # Handle commands or text messages
        if text == '/start':
            send_message(chat_id, "Hello! I'm your bot.")
        else:
            send_message(chat_id, f"You said: {text}")

    return jsonify({'status': 'ok'})

if __name__ == '__main__':
    # Set webhook URL to Telegram. Replace <your-webhook-url> with your actual public URL
    webhook_url = f"https://<your-webhook-url>/webhook"
    set_webhook_url = f"{TELEGRAM_API_URL}/setWebhook?url={webhook_url}"
    
    # Set the webhook by making a direct API call to Telegram
    webhook_response = requests.get(set_webhook_url)
    
    if webhook_response.status_code == 200:
        print("Webhook set successfully")
    else:
        print("Failed to set webhook")
    
    # Run the Flask app
    app.run(port=5000)
```

### Step 2: Deploy the Flask Application
You can run this locally with Ngrok or deploy it to a cloud service to get a public URL.

### Explanation:

1. **Webhook Endpoint**:
   - The `/webhook` route is the endpoint that Telegram will send updates to.
   - We extract the `chat_id` and `text` from the incoming message and handle basic responses. 

2. **Direct API Call**:
   - Instead of using `python-telegram-bot` or any dispatcher, we use the `requests` library to send HTTP POST requests to the Telegram Bot API.
   - The `send_message` function is responsible for sending messages to Telegram users.

3. **Setting the Webhook**:
   - Before running the Flask app, we set the webhook URL by making a direct GET request to Telegram's API using `requests.get()`.
   - Replace `https://<your-webhook-url>/webhook` with your actual public URL. If you're using Ngrok, it will look something like `https://abc123.ngrok.io/webhook`.

### Step 3: Running the Application
1. Start the Flask application:
   ```bash
   python app.py
   ```

2. If using Ngrok, start it in a separate terminal:
   ```bash
   ngrok http 5000
   ```

3. Ensure your webhook is correctly set up by checking Telegram's response after setting the webhook.

### Step 4: Testing the Bot
1. Open Telegram and send `/start` to your bot. The bot should respond with "Hello! I'm your bot."
2. Send any other text, and the bot should echo it back with a message like "You said: <your text>."

### Summary:
- **Direct API Calls**: This approach uses raw HTTP requests to interact with the Telegram Bot API.
- **Flask for Webhooks**: The bot is served using Flask, and webhooks are handled by the `/webhook` route.
- **Flexible Integration**: This approach allows more direct control over Telegram's API and can be adapted to other frameworks or environments.

Let me know if you need further clarification or encounter any issues!

- GitHub Example:
