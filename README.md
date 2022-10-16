# Panic-Bot
Setting up node monitoring using Panic Bot

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
