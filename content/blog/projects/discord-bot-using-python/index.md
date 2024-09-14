---
title: Build a Discord bot using Python
date: "2024-08-29T22:40:32.169Z"
description: How to create an interractive Discord bot.
---

To start your Discord bot, follow these steps:

1. **Install Discord.py**: Ensure you have the `discord.py` library installed. You can do this using pip:

   ```bash
   pip install discord.py
   ```

2. **Create a Bot Script**: Create a Python file (e.g., `bot.py`) with the following basic structure:

   ```python
   import discord

   # Set up logging
   import logging
   logging.basicConfig(level=logging.INFO)

   # Initialize the client
   client = discord.Client()

   # Event when the bot is ready
   @client.event
   async def on_ready():
       print(f'We have logged in as {client.user}')

   # Run the bot
   client.run('your_token_here')
   ```

   Replace `'your_token_here'` with your actual Discord bot token.

3. **Run the Bot**: Execute your script in the terminal:

   ```bash
   python bot.py
   ```

4. **Check Your Bot**: Once the script is running, your bot should appear online in the Discord server it's invited to.

Make sure to keep the terminal open while your bot is running. If you close it, the bot will go offline.

#### Troubleshooting Errors

```
TypeError: Client.__init__() missing 1 required keyword-only argument: 'intents'
```

The error indicates that the `intents` argument is missing when initializing the `Client`. You need to specify the intents your bot will use. Here's how you can fix it:

1. **Import Intents**: Import `discord.Intents`.

2. **Set Up Intents**: Enable the necessary intents for your bot.

3. **Initialize Client with Intents**: Pass the `intents` to the `Client`.

Here's an example:

```python
import discord
import logging

# Set up logging
logging.basicConfig(level=logging.INFO)

# Set up intents
intents = discord.Intents.default()
intents.message_content = True  # Enable additional intents as needed

# Initialize the client with intents
client = discord.Client(intents=intents)

# Event when the bot is ready
@client.event
async def on_ready():
    print(f'We have logged in as {client.user}')

# Run the bot
client.run('your_token_here')
```

Make sure to replace `'your_token_here'` with your actual token and adjust the intents according to your needs. For instance, enable `intents.guilds`, `intents.members`, or others depending on what your bot needs to do.

```
File "/home/mary/workspace/bot_buddy/bot-env/lib/python3.10/site-packages/discord/http.py", line 758, in request
    raise HTTPException(response, data)
discord.errors.HTTPException: 401 Unauthorized (error code: 0): 401: Unauthorized
```

The `401 Unauthorized` error indicates that the bot token is invalid or not authorized. Here’s how to troubleshoot this issue:

1. **Verify the Token**: Double-check that the token is correctly copied from the Discord Developer Portal. Ensure there are no extra spaces or missing characters.

2. **Regenerate the Token**: If necessary, regenerate the token in the Discord Developer Portal and update your script with the new token.

3. **Environment Variables**: If using environment variables, print them in your script to confirm they are correctly set:

   ```python
   import os
   print(os.getenv('YOUR_ENV_VAR_NAME'))
   ```

4. **Token Privacy**: Ensure the token is not exposed in version control or logs, as it can be invalidated if compromised.

5. **Reinvite the Bot**: Make sure the bot is invited to the server with the correct permissions.

6. **Library Version**: Update the `discord.py` library to the latest version:

   ```bash
   pip install -U discord.py
   ```

7. **Check Discord API Status**: Visit the Discord status page to ensure there are no ongoing issues with their API.

If the problem persists, double-check each step and try running a minimal script to isolate the issue.

```
raise PrivilegedIntentsRequired(exc.shard_id) from None
discord.errors.PrivilegedIntentsRequired: Shard ID None is requesting privileged intents that have not been explicitly enabled in the developer portal. It is recommended to go to https://discord.com/developers/applications/ and explicitly enable the privileged intents within your application's page. If this is not possible, then consider disabling the privileged intents instead.
```

To resolve the `PrivilegedIntentsRequired` error, follow these steps:

1. **Visit the Discord Developer Portal**: Go to [Discord Developer Portal](https://discord.com/developers/applications/).

2. **Select Your Application**: Click on your bot application.

3. **Navigate to "Bot" Settings**: In the left sidebar, select "Bot".

4. **Enable Privileged Intents**: Under the "Privileged Gateway Intents" section, enable the intents you need, such as:

   - **Presence Intent**: If your bot needs to track user presence.
   - **Server Members Intent**: If your bot needs to access member data.

5. **Save Changes**: Don't forget to save your changes.

6. **Update Your Code**: Ensure your code requests the necessary intents. For example:

   ```python
   intents = discord.Intents.default()
   intents.members = True  # Enable this if you need member data

   client = discord.Client(intents=intents)
   ```

7. **Restart Your Bot**: Restart your bot to apply the changes.

By doing this, you ensure your bot has the permissions it needs to function properly.

### Adding Commands to Discord Bot

To add a `ping` command to your existing bot code, you should use the `commands.Bot` class from `discord.ext.commands` instead of `discord.Client`. The `commands.Bot` class provides a built-in command framework that simplifies adding and managing commands.

Here’s how you can modify your existing code to use `commands.Bot` and add a `ping` command:

### Updated Code with `commands.Bot`

**`bot.py`**

```python
import discord
from discord.ext import commands
import logging

# Set up logging
logging.basicConfig(level=logging.INFO)

# Set up intents
intents = discord.Intents.default()
intents.message_content = True  # Enable additional intents as needed

# Initialize the bot with a command prefix and intents
bot = commands.Bot(command_prefix='!', intents=intents)

# Event when the bot is ready
@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

# Define a ping command
@bot.command(name='ping')
async def ping(ctx):
    # Send a message with the bot's latency
    await ctx.send(f'Pong! Latency is {round(bot.latency * 1000)}ms')

# Define a hello command
@bot.command(name='hello')
async def hello(ctx):
    await ctx.send('Hello!')

# Run the bot
bot.run('YOUR_BOT_TOKEN')
```

### Changes and Additions

1. **Import `commands`**:

   - Import `commands` from `discord.ext`.

2. **Initialize `commands.Bot`**:

   - Replace `discord.Client` with `commands.Bot`, providing a command prefix (e.g., `!`).

3. **Add a `ping` Command**:

   - Use the `@bot.command` decorator to define the `ping` command.
   - The `ping` command responds with the bot’s latency.

4. **Add the `hello` Command**:

   - Move the `hello` functionality to a command using the `@bot.command` decorator.

5. **Run the Bot**:
   - Replace `'YOUR_BOT_TOKEN'` with your actual bot token.

### Token Security

**Important**: Never share your bot token publicly. Make sure to keep it secure. Consider using environment variables to manage sensitive information such as bot tokens.

You can store your token in an environment variable and access it using `os.environ`. Here’s an example of how you might update your code to use an environment variable:

**Update for Environment Variable**:

1. **Install `python-dotenv`** (optional for loading environment variables from a `.env` file):

   ```bash
   pip install python-dotenv
   ```

2. **Create a `.env` File**:

   **`.env`**

   ```
   DISCORD_BOT_TOKEN=YOUR_BOT_TOKEN
   ```

3. **Update Your Code to Use the Environment Variable**:

   **`bot.py`**

   ```python
   import discord
   from discord.ext import commands
   import logging
   import os
   from dotenv import load_dotenv

   # Load environment variables from .env file
   load_dotenv()

   # Get the bot token from the environment
   TOKEN = os.getenv('DISCORD_BOT_TOKEN')

   # Set up logging
   logging.basicConfig(level=logging.INFO)

   # Set up intents
   intents = discord.Intents.default()
   intents.message_content = True  # Enable additional intents as needed

   # Initialize the bot with a command prefix and intents
   bot = commands.Bot(command_prefix='!', intents=intents)

   # Event when the bot is ready
   @bot.event
   async def on_ready():
       print(f'We have logged in as {bot.user}')

   # Define a ping command
   @bot.command(name='ping')
   async def ping(ctx):
       # Send a message with the bot's latency
       await ctx.send(f'Pong! Latency is {round(bot.latency * 1000)}ms')

   # Define a hello command
   @bot.command(name='hello')
   async def hello(ctx):
       await ctx.send('Hello!')

   # Run the bot
   bot.run(TOKEN)
   ```

By following these instructions, you will be able to add a `ping` command to your Discord bot and enhance its functionality while using `commands.Bot` for command management.

### Integrating OpenAI

Run `pip install openai==0.28`

#### Modify Code

```
import discord
from discord.ext import commands
import logging
import os
from dotenv import load_dotenv

import openai

# from openai import OpenAI

# client = OpenAI(
#     # Defaults to os.environ.get("OPENAI_API_KEY")
# )

# chat_completion = client.chat.completions.create(
#     model="gpt-4o-mini",
#     messages=[{"role": "user", "content": "Hello world"}]
# )

# Load environment variables from .env file
load_dotenv()

# Get the bot token from the environment
TOKEN = os.getenv('DISCORD_BOT_TOKEN')
OPENAI_API_KEY = os.getenv('OPENAI_API_KEY')

# Set up logging
logging.basicConfig(level=logging.INFO)

# Set up intents
intents = discord.Intents.default()
intents.message_content = True  # Enable additional intents as needed

# Initialize the bot with a command prefix and intents
bot = commands.Bot(command_prefix='!', intents=intents)

# Event when the bot is ready
@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

# Define a ping command
@bot.command(name='ping')
async def ping(ctx):
    # Send a message with the bot's latency
    await ctx.send(f'Pong! Latency is {round(bot.latency * 1000)}ms')

# Define a hello command
@bot.command(name='hello')
async def hello(ctx):
    await ctx.send('Hello!')

# Define the 'ask' command
@bot.command(name='ask')
async def ask(ctx):
    user_ask_mode[ctx.author.id] = True
    await ctx.send('Fire away!')

# Track whether the user is in 'ask' mode
user_ask_mode = {}

# Set OpenAI API key
openai.api_key = OPENAI_API_KEY

# Handle messages
@bot.event
async def on_message(message):
    if message.author == bot.user:
        return

    if message.content.startswith('!'):
        await bot.process_commands(message)
        return

    if user_ask_mode.get(message.author.id):
        # Send the user's message to ChatGPT
        response = openai.Completion.create(
            engine="text-davinci-003",  # You can choose the appropriate engine
            prompt=message.content,
            max_tokens=150
        )
        chatgpt_reply = response.choices[0].text.strip()
        await message.channel.send(chatgpt_reply)

        # Disable 'ask' mode for the user after receiving a response
        user_ask_mode[message.author.id] = False


# Run the bot
bot.run(TOKEN)

```

### Storing Conversation History to Maintain Context

To enable continuous conversation with ChatGPT, you'll need to maintain a conversation history for each user. This way, each message from the user can be appended to the existing conversation, and ChatGPT can generate responses based on the full context.

Here's how you can update your bot to support ongoing conversations with ChatGPT:

1. **Maintain Conversation History**: Store a list of messages for each user, which will be sent to ChatGPT with each new message.

2. **Handle Conversation State**: Update the state management to keep track of ongoing conversations.

### Updated Code

Here's how you can adjust your code to handle continuous conversations:

**`bot.py`**

```python
import discord
from discord.ext import commands
import logging
import os
from dotenv import load_dotenv
import openai

# Load environment variables
load_dotenv()

# Get API keys from environment
DISCORD_TOKEN = os.getenv('DISCORD_BOT_TOKEN')
OPENAI_API_KEY = os.getenv('OPENAI_API_KEY')

# Set up logging
logging.basicConfig(level=logging.INFO)

# Set up intents
intents = discord.Intents.default()
intents.message_content = True  # Enable additional intents as needed

# Initialize the bot with a command prefix and intents
bot = commands.Bot(command_prefix='!', intents=intents)

# Set up OpenAI API client
openai.api_key = OPENAI_API_KEY

# Dictionary to track conversation history for each user
user_conversations = {}

# Track whether the user is in 'ask' mode
user_ask_mode = {}

# Event when the bot is ready
@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

# Define a ping command
@bot.command(name='ping')
async def ping(ctx):
    # Send a message with the bot's latency
    await ctx.send(f'Pong! Latency is {round(bot.latency * 1000)}ms')

# Define a hello command
@bot.command(name='hello')
async def hello(ctx):
    await ctx.send('Hello!')

# Define the 'ask' command
@bot.command(name='ask')
async def ask(ctx):
    user_ask_mode[ctx.author.id] = True
    user_conversations[ctx.author.id] = []  # Initialize conversation history
    await ctx.send('Fire away!')

# Handle messages
@bot.event
async def on_message(message):
    if message.author == bot.user:
        return

    if message.content.startswith('!'):
        await bot.process_commands(message)
        return

    if user_ask_mode.get(message.author.id):
        # Append user message to conversation history
        if message.author.id not in user_conversations:
            user_conversations[message.author.id] = []

        user_conversations[message.author.id].append({"role": "user", "content": message.content})

        # Send the conversation history to ChatGPT
        response = openai.ChatCompletion.create(
            model="gpt-4",  # Or another model like "gpt-3.5-turbo"
            messages=user_conversations[message.author.id]
        )

        # Get the ChatGPT response
        chatgpt_reply = response.choices[0].message['content']
        await message.channel.send(chatgpt_reply)

        # Append ChatGPT's reply to the conversation history
        user_conversations[message.author.id].append({"role": "assistant", "content": chatgpt_reply})

        # Optionally, clear the conversation history after a certain condition
        # user_ask_mode[message.author.id] = False

# Run the bot
bot.run(DISCORD_TOKEN)
```

### Explanation

1. **Environment Variables**:

   - Ensure you load your OpenAI API key from an environment variable.

2. **Conversation History**:

   - `user_conversations` dictionary keeps track of each user's conversation history with ChatGPT.

3. **Handling Messages**:

   - When a user sends a message in 'ask' mode, it's appended to their conversation history.
   - The entire conversation history is sent to ChatGPT to maintain context.
   - ChatGPT's response is added to the conversation history and sent back to the user.

4. **Clearing Conversation History**:
   - Depending on your needs, you may want to clear or reset the conversation history after a certain condition. You can uncomment the line `user_ask_mode[message.author.id] = False` to reset the state if you want to stop the conversation after a specific condition.

### Running Your Bot

Run the bot with:

```bash
python bot.py
```

With this setup, users can continue chatting with ChatGPT. The bot maintains context across multiple messages by sending the full conversation history with each request. This allows ChatGPT to provide more coherent and contextually appropriate responses.

### Using a Database to Store Conversation History

Yes, you can use a database to store user conversations, which is especially useful for handling large amounts of data or maintaining state across bot restarts. By storing conversations in a database, you ensure that the context is preserved even if the bot is restarted or crashes.

Here’s a step-by-step guide on how to integrate a database for storing user conversations with your Discord bot:

### 1. Choose a Database

For simplicity, I'll use MongoDB as an example because it's flexible and widely used for such tasks. However, you can use any database you prefer (e.g., PostgreSQL, SQLite, MySQL).

### 2. Set Up MongoDB

1. **Install MongoDB**: You can use a managed service like MongoDB Atlas, or set up a local MongoDB instance.

2. **Install MongoDB Client for Python**:
   Install `pymongo` to interact with MongoDB from Python:

   ```bash
   pip install pymongo
   ```

### 3. Update Your Bot Script

Here's how you can modify your bot script to use MongoDB to store user conversations.

**`bot.py`**

```python
import discord
from discord.ext import commands
import logging
import os
from dotenv import load_dotenv
import openai
from pymongo import MongoClient

# Load environment variables
load_dotenv()

# Get API keys and MongoDB URI from environment
DISCORD_TOKEN = os.getenv('DISCORD_BOT_TOKEN')
OPENAI_API_KEY = os.getenv('OPENAI_API_KEY')
MONGODB_URI = os.getenv('MONGODB_URI')  # e.g., 'mongodb://localhost:27017/'

# Set up logging
logging.basicConfig(level=logging.INFO)

# Set up intents
intents = discord.Intents.default()
intents.message_content = True  # Enable additional intents as needed

# Initialize the bot with a command prefix and intents
bot = commands.Bot(command_prefix='!', intents=intents)

# Set up OpenAI API client
openai.api_key = OPENAI_API_KEY

# Set up MongoDB client
mongo_client = MongoClient(MONGODB_URI)
db = mongo_client['discord_bot']  # Database name
conversations_collection = db['conversations']  # Collection name

# Track whether the user is in 'ask' mode
user_ask_mode = {}

# Event when the bot is ready
@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

# Define a ping command
@bot.command(name='ping')
async def ping(ctx):
    # Send a message with the bot's latency
    await ctx.send(f'Pong! Latency is {round(bot.latency * 1000)}ms')

# Define a hello command
@bot.command(name='hello')
async def hello(ctx):
    await ctx.send('Hello!')

# Define the 'ask' command
@bot.command(name='ask')
async def ask(ctx):
    user_ask_mode[ctx.author.id] = True
    # Initialize conversation history in the database
    conversations_collection.update_one(
        {'user_id': ctx.author.id},
        {'$set': {'messages': []}},
        upsert=True
    )
    await ctx.send('Fire away!')

# Handle messages
@bot.event
async def on_message(message):
    if message.author == bot.user:
        return

    if message.content.startswith('!'):
        await bot.process_commands(message)
        return

    if user_ask_mode.get(message.author.id):
        # Retrieve the user's conversation history from the database
        user_conversation = conversations_collection.find_one({'user_id': message.author.id})
        if user_conversation:
            conversation_history = user_conversation.get('messages', [])
        else:
            conversation_history = []

        # Append user message to conversation history
        conversation_history.append({"role": "user", "content": message.content})

        # Send the conversation history to ChatGPT
        response = openai.ChatCompletion.create(
            model="gpt-4",  # Or another model like "gpt-3.5-turbo"
            messages=conversation_history
        )

        # Get the ChatGPT response
        chatgpt_reply = response.choices[0].message['content']
        await message.channel.send(chatgpt_reply)

        # Append ChatGPT's reply to the conversation history
        conversation_history.append({"role": "assistant", "content": chatgpt_reply})

        # Update the conversation history in the database
        conversations_collection.update_one(
            {'user_id': message.author.id},
            {'$set': {'messages': conversation_history}}
        )

        # Optionally, clear the ask mode
        # user_ask_mode[message.author.id] = False

# Run the bot
bot.run(DISCORD_TOKEN)
```

### Explanation

1. **MongoDB Setup**:

   - **`MongoClient`**: Connect to MongoDB using the URI stored in environment variables.
   - **`db` and `conversations_collection`**: Select the database and collection where conversations will be stored.

2. **Conversation Handling**:

   - **`ask` Command**: Initializes the conversation history for the user in the database.
   - **`on_message` Event**: Retrieves the user's conversation history from the database, appends the user's message, sends it to ChatGPT, and updates the conversation history with the response.

3. **Handling Messages**:
   - **Retrieve and Update**: Fetch the conversation history from MongoDB, update it with new messages, and store it back in the database after generating a response.

### Notes

- **Database Security**: Ensure your MongoDB instance is properly secured and not exposed publicly.
- **Error Handling**: Consider adding error handling to manage issues with database connectivity or OpenAI API requests.
- **Scalability**: Depending on your needs, you may need to optimize the database schema or handle large-scale data more efficiently.

By using MongoDB (or any other database), you can efficiently manage and persist user conversations, ensuring a smooth and consistent user experience for your Discord bot.
