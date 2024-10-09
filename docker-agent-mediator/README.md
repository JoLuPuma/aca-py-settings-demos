# Running aca-py
This directory contains scripts to run an aca-py agent mediator loading its arguments from a specified file  using ```--arg-file```.  This file must be in YAML format.
List of files included:
-   mediateur-auto-accept.yml

> This demo only illustrates the basic configuration for an agent running as a mediator. There is no reverse proxy (as Caddy) configured. If you want to run a mediator ready-to-test with a mobile application you can go and check the [Aries  Mediator Service.](https://github.com/hyperledger/aries-mediator-service)

## Before running the docker compose

Given that this demo use Ngrok, you have to get an Ngrok token and set it in the `env.` file 
- You can create the `.env` file from the example in this repo. And
- set your Ngrok token in  the variable NGROK_AUTHTOKEN
