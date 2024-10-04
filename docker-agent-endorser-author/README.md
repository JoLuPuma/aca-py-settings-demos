
# Running an Endorser and Author Demo
These two agents (Endorser and Author) have DIDs and Verkeys already publish in the ledger. These two agents have and set the respective SEEDs that were used to create the DIDs and Verkeys already mentioned. 

## 1. Starting an Author agent with automatic endorsement 
First start the endorser. The endorser agent generates by default an invitation. This invitation is going to be used for setting the Author agent.
### Endorser part
1.  Set the config file ```endorser-auto.yml``` in the ```docker-compose.yml``` file like this ```--arg-file './configs/endorser-auto.yml'```
2.  Start the endorser with ```docker compose up ``` 
3.  Look for the invitation in the terminal. It must look like this:
 
    ```http://host.docker.internal:3003?c_i=eyJAdHlwZSI6ICJodHRwczovL2RpZGNvbW0ub3JnL2Nvbm5lY3Rpb25zLzEuMC9pbnZpdGF0aW9uIiwgIkBpZCI6ICI2NTMzM2JmZC03YTk0LTRmYzEtOGUyMy1lZWIzY2YxOGNiNzciLCAibGFiZWwiOiAiQWNjclx1MDBlOWRpdGV1ciIsICJyZWNpcGllbnRLZXlzIjogWyJHcmJhMUx0NExSZDdxZ2t4blVkSjRDcW9neHdkamF6bWs2MXlDV1ZudmZRbyJdLCAic2VydmljZUVuZHBvaW50IjogImh0dHA6Ly9ob3N0LmRvY2tlci5pbnRlcm5hbDozMDAzIn0=```

4.  If there is no errors, we're ready to endorse transactions

### Author part
5.  Copy and paste the invitation (from step 3) in the config file of the Author (in this case we're going to use the ```author-auto.yml``` file). In order to automatically set the endorsement the Author agent must know the public DID of the Endorser Agent . Make sure the following parameters are correctly set (***Replace the endorser-public-did value with your Endorser public DID*** ).
    ```yml 
    endorser-protocol-role: author
    endorser-public-did: R1SbxVKLcBRSHj9s55MAGf
    endorser-alias: Accréditeur
    endorser-invitation: http://host.docker.internal:3003?c_i=eyJAdHlwZSI6ICJodHRwczovL2RpZGNvbW0ub3JnL2Nvbm5lY3Rpb25zLzEuMC9pbnZpdGF0aW9uIiwgIkBpZCI6ICI2NTMzM2JmZC03YTk0LTRmYzEtOGUyMy1lZWIzY2YxOGNiNzciLCAibGFiZWwiOiAiQWNjclx1MDBlOWRpdGV1ciIsICJyZWNpcGllbnRLZXlzIjogWyJHcmJhMUx0NExSZDdxZ2t4blVkSjRDcW9neHdkamF6bWs2MXlDV1ZudmZRbyJdLCAic2VydmljZUVuZHBvaW50IjogImh0dHA6Ly9ob3N0LmRvY2tlci5pbnRlcm5hbDozMDAzIn0=
    auto-request-endorsement: true
    auto-write-transactions: true
    auto-create-revocation-transactions: true
    ```

 6. Set the config file ```author-auto.yml``` in the ```docker-compose-author.yml``` file like this ```--arg-file './configs/author-auto.yml'```
 7. Start the Author agent ```docker compose -f docker-compose-author.yml up``` 
 8. And that's all . Now we can test the set up. If there is no error in both agents we can try publishing the following schema  to the ledger :
 Call the ```POST /anoncreds/schema``` with this body object:


```json
    {
       "schema": {
        "attrNames": [
          "name","last_name"
        ],
        "issuerId": "W9o4XdJtB6RKqMyHGvcQn5",
        "name": "IdentityTest",
        "version": "1.0"
      }
    }
    ```
## 2. Starting an Author agent with manual endorsement 
TODO