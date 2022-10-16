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
```
sudo systemctl enable redis-server.service
```
# Installing Panic:
```
adduser panic --disabled-login
```
#Press Enter 5 times, after - y

# Create a folder and change permissions:
```
mkdir /opt/panic
chown -R panic:panic /opt/panic
su panic
```
