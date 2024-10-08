
This demos illustrates the deployment pattern Provision/Start of an ACA-PY agent.

The `docker-compose.yml`file it's sef-explanatory. So, give it a look
The `provision.yml` file contains the secrets for provisioning the agent
The `start.yml` file contains the secrets to access to the wallet and operate the agent.

Before running the docker compose, make sure to set the following variables:
- seed (in the yml file)
- ACAPY_GENESIS_URL
- ACAPY_TAILS_SERVER_BASE_URL  