# Panic-Bot
Setting up node monitoring using Panic Bot

On the server on which we will install and configure the bot, we perform the following steps

# Updating packages:

```
sudo apt update && sudo apt upgrade -y
```

# Setting up PIP Package Manager and Redis Database Server:

```
sudo apt-get install python3-pip redis-server -y
```

# Install Pipenv Packaging Tool:

```
sudo pip3 install pipenv
sudo systemctl enable redis-server.service
```

# Installing Panic:

```
adduser panic --disabled-login
```
(Press Enter 5 times, after - y)

# Create a folder and change permissions(one command):

```
mkdir /opt/panic && \
chown -R panic:panic /opt/panic && \
su panic
```

# Enter to the folder and clone the repository(one command):

```
cd /opt/panic/ && \
git clone https://github.com/SimplyVC/panic_cosmos.git && \
cd panic_cosmos && \
git checkout master
```
# Updating the packages and running the Panic setup(one command):

```
pipenv update && \
pipenv run python run_setup.py
```
(Enter a unique identifier for notifications)

# Setting up a telegram bot

Set up telegram alerts by pressing Y
Next, we need to enter the API token:

* We go to telegram
* In the search, enter @BotFather, go to the bot and click Start
* Use the /newbot command to create a bot, enter the bot name and username. The username must end with bot (eg x3m_bot). After that, BotFather will send a message with a link to our bot and an API token. Copy the API token and save it in notepad
* Follow the link to the chat of your bot and click Start
* Follow the link api.telegram.org/bot<token>/getUpdates, replacing <token> with your bot's API token. Copy id and save to notepad

Now that we have the bot, API token, and chat ID, let's go back to the terminal and continue setting up Panic!:
  
 * Insert the API token and press Enter
 * Paste the chat ID and press Enter
 * Press Y to check alerts. We go to telegram and check the notification. Upon successful setup, a message will appear - Test Alert
 * Press Y again, confirming that the notification has arrived
  
  Next Panic! asks if other alerts (e-mail, phone calls or periodic alerts) are needed. Since we are setting up only the telegram bot, we answer three requests with n
  
 # Setting up commands for telegram bot
  
  * Press Y
  * Insert the API token and press Enter
  * Paste the chat ID and press Enter
  * Press Y to test bot commands
  * Go to the telegram bot and enter the command /ping Successful response - PONG!
  * Press Y again, confirming that the command was successfully completed
  
# Setting up Redis
  
  To configure Redis, we will use values:
  
  * Redis host IP: localhost
  * Redis host port : 6565
  * Redis password: no password
    
  Confirm Y and Enter three times
  Testing Redis сonfirm Y
  
# Connecting a node to monitoring
  
  We go to the server with the node that we want to connect to the bot and make changes to the config file. Use your path to the config file (usually $HOME/.project_name/config/config.toml)
  
  ```
  nano ~/.haqqd/config/config.toml
  ```
  
  In the TCP or UNIX socket address for the RPC server to listen on block, change the value of laddr:
  
  ```
  laddr = "tcp://0.0.0.0:26657"
  ```
  
  Save changes with a keyboard shortcut: Ctrl + X Y Enter  
  If Panic! and Redis are installed on the same server with the node, then we leave it unchanged
  
  ```
  laddr = "tcp://127.0.0.1:26657"
  ```
  
  Restart the node:
  
  ```
  sudo systemctl restart haqq
  ```
  
  Back to the server with Panic!
  Enter the name of the node and the RPC url in the format http://NODE_IP:26657, confirm that the node is a validator.
  To configure notifications about changes to github repositories, edit the file user_config_repos.in:
  
  ```
  nano /opt/panic/panic_cosmos/config/user_config_repos.in
  ```
  
  # The launch of Panic!
  
  Create a service file:
  
  ```
  tee /etc/systemd/system/panic.service > /dev/null <<EOF
  [Unit]
Description=P.A.N.I.C.
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
User=panic
TimeoutStopSec=90s
WorkingDirectory=/opt/panic/panic_cosmos
ExecStart=/usr/local/bin/pipenv run python /opt/panic/panic_cosmos/run_alerter.py

[Install]
WantedBy=multi-user.target
  EOF
  
  ```
  
Start the service and check the logs
  
  ```
sudo systemctl daemon-reload
sudo systemctl enable panic
sudo systemctl start panic
sudo journalctl -u panic -f
  ```
  
  To add other nodes, edit the user_config_nodes.ini file, having previously made changes to the config.toml file of the node we want to add
  In the user_config_nodes.ini file, copy all the existing lines and paste below. Change the number, name and url. Save the changes. Then restart Panic! 
  Check that the second node appears in the logs
