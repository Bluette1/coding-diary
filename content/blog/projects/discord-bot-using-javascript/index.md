---
title: Create a Discord Bot using Javascript
date: "2024-10-17T22:40:32.169Z"
description: How to Create a Discord Bot using Javascript.
---

Creating a Discord bot involves the following steps, which will guide you through setting up the bot in the **Discord Developer Portal**, writing basic bot code using **Node.js** and the **discord.js** library, and running the bot.

### Prerequisites
Before you begin, ensure you have the following:
- **Node.js** installed. [Download Node.js](https://nodejs.org/)
- **Basic coding knowledge** (JavaScript in this case).
- A **Discord account**.
- A **code editor** (e.g., [Visual Studio Code](https://code.visualstudio.com/)).

Now, let’s begin!

## Step 1: Create a Discord Application (Bot)

1. **Go to the Discord Developer Portal**:  
   Open [Discord Developer Portal](https://discord.com/developers/applications) and log in with your Discord account.

2. **Create a New Application**:
   - Click on the **New Application** button.
   - Give your application a name (this will be your bot's name).
   - Click **Create**.

3. **Create a Bot**:
   - In the application dashboard, go to the **Bot** tab on the left sidebar.
   - Click **Add Bot** and confirm.
   - Now, your bot is created. Under **Username**, you will see the bot’s username and token.

4. **Copy the Token** (Important!):
   - Under the **TOKEN** section, click **Copy**. This is the token that your bot will use to authenticate with Discord. Keep this token secure and **do not share it** with anyone.

5. **Set Permissions** for the Bot:
   - Scroll down to the **Privileged Gateway Intents** section. For basic bots, you can leave these unchecked. However, if your bot will use features like member updates or message content (for moderation purposes), you may need to enable **Server Members Intent** and/or **Message Content Intent**.

6. **Invite the Bot to Your Server**:
   - Go to the **OAuth2** tab, then select **URL Generator**.
   - Under **Scopes**, check the box for `bot`.
   - Scroll down to **Bot Permissions** and select the permissions your bot will need (e.g., `Send Messages`, `Manage Messages`, `Kick Members`, etc.).
   - Copy the generated URL and paste it into your browser to invite the bot to a server where you have admin privileges.

Now your bot is set up in Discord and added to your server!

---

## Step 2: Set Up Your Development Environment

1. **Install Node.js**:
   If you haven't already, download and install Node.js from [nodejs.org](https://nodejs.org/).

2. **Create a Project Folder**:
   Open your terminal (or command prompt), and create a new folder where you'll store your bot's files.

   ```bash
   mkdir discord-bot
   cd discord-bot
   ```

3. **Initialize Node.js**:
   In the terminal, initialize a new Node.js project by running:

   ```bash
   npm init -y
   ```

   This will create a `package.json` file in your project directory.

4. **Install discord.js**:
   Install the **discord.js** library, which allows you to interact with the Discord API.

   ```bash
   npm install discord.js
   ```

---

## Step 3: Write the Bot Code

1. **Create a File for Your Bot**:
   Inside your project folder, create a file called `index.js` (or any name you prefer). This will be your bot’s main script.

2. **Write Basic Code**:
   Open `index.js` in your code editor, and add the following code to get a basic bot running:

   ```javascript
   // Require the discord.js module
   const { Client, GatewayIntentBits } = require('discord.js');

   // Create a new client instance
   const client = new Client({
     intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent],
   });

   // When the client is ready, run this code (only once)
   client.once('ready', () => {
     console.log(`Logged in as ${client.user.tag}!`);
   });

   // Listen for messages
   client.on('messageCreate', message => {
     // Ignore messages from bots
     if (message.author.bot) return;

     // Respond to specific messages
     if (message.content.toLowerCase() === 'hello bot') {
       message.reply('Hello, human!');
     }
   });

   // Log in to Discord with your app's token
   client.login('YOUR_BOT_TOKEN_HERE');
   ```

   Replace `'YOUR_BOT_TOKEN_HERE'` with the token you copied earlier from the Discord Developer Portal.

### Explanation:
- **Client**: This is your bot’s interface with Discord.
- **Events**: The bot listens for events like `ready` (when the bot starts) and `messageCreate` (when a message is sent in a server).
- **message.reply()**: This is how the bot responds to messages it receives.

---

## Step 4: Run Your Bot

1. **Save your script** (`index.js`).
2. In the terminal, run your bot:

   ```bash
   node index.js
   ```

   You should see a message in the terminal saying something like:

   ```
   Logged in as YourBotName#1234!
   ```

3. **Test the Bot**:
   - Go to your Discord server.
   - Type `hello bot` in one of the channels where the bot has permissions.
   - The bot should reply with `Hello, human!`.

---

## Step 5: Keep the Bot Running (Optional)

If you want your bot to stay online 24/7, you’ll need to host it somewhere, such as:
- **Heroku** (Free tier can work for small bots).
- **VPS** (like DigitalOcean or AWS Lightsail).
- **Dedicated Bot Hosting** services.

For local testing, you can simply run the bot on your computer, but the bot will go offline when you close the terminal.

---

## Next Steps: Expanding Your Bot

Now that you have a basic bot up and running, you can extend its functionality by adding more commands and features. Here are some ideas:
- **Moderation Commands**: Add commands for kicking, banning, and muting users.
- **Fun Commands**: Create commands for jokes, memes, or games.
- **Music Bot**: Add the ability to play music from YouTube or other sources (requires more advanced libraries like `@discordjs/voice`).

You can refer to the [discord.js documentation](https://discord.js.org/#/) for more advanced features and event handling.

---

### Summary of Steps:
1. Create a bot in the **Discord Developer Portal**.
2. Set up a **Node.js project** and install **discord.js**.
3. Write basic bot code to respond to messages.
4. Run your bot locally and test it in your server.

### Points to Note/Remember:
Here are the key points to remember regarding setting up your Discord bot, particularly when it comes to channel IDs and intents:

### Important Points to Note

1. **Enable Developer Mode**:
   - Go to Discord User Settings → Advanced → Enable **Developer Mode**. This allows you to right-click on channels, servers, and users to copy their IDs easily.

2. **Channel IDs**:
   - After enabling Developer Mode, you can obtain the channel ID by right-clicking on the desired channel and selecting "Copy ID." Make sure the ID is numeric and correctly defined in your code.

3. **Intents**:
   - Different events require specific intents to be enabled. For example:
     - **Server Members Intent**: Required for the `on_member_join` event.
     - **Message Content Intent**: Needed if you want to read message contents.
   - **Enable Intents in the Developer Portal**:
     - Navigate to the Discord Developer Portal, select your application, then go to the **Bot** section. Under **Privileged Gateway Intents**, enable the required intents.

4. **Intents in Code**:
   - When creating your bot instance, ensure you've enabled the necessary intents in your code:

   ```python
   intents = discord.Intents.default()
   intents.members = True  # Enable members intent
   intents.message_content = True  # Enable message content intent if needed

   bot = commands.Bot(command_prefix='!', intents=intents)
   ```

5. **Bot Permissions**:
   - Make sure your bot has the necessary permissions within the server to read messages, send messages, and manage roles if needed. Check both the role settings and channel-specific permissions.

6. **Debugging**:
   - If you encounter issues, use print statements or logging to track the flow of your code. This can help identify where things might be going wrong.

7. **Rate Limits**:
   - Be aware of Discord's rate limits. If your bot sends too many messages in a short period, it may be temporarily restricted.


