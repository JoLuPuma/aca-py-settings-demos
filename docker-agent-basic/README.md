# Running aca-py
This directory contains scripts to run an aca-py agent loading its arguments from a specified file  using ```--arg-file```.  This file must be in YAML format.
List of file included:
-   default.yml
-   endorser.yml

## Before running the docker compose

Given that this demo use Ngrok, you have to get an Ngrok token and set it in the `env.` file 
- You can create the `.env` file from the example in this repo. And
- set your Ngrok token in  the variable NGROK_AUTHTOKEN

Also, make sure to set the following variables  in the yaml file:
- seed
- genesis-url
