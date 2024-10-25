
# Running an Endorser and Author Demo
These two agents (Endorser and Author) have DIDs and Verkeys already publish in the ledger. These two agents have and set the respective SEEDs that were used to create the DIDs and Verkeys already mentioned. 

## 1. Starting an Author agent with automatic endorsement 
We're going to start the Author agent with an invitation previously created by the Endorser Agent using the `endorser-invitation` parameter. 

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
        "issuerId": "put-your-did-here",
        "name": "IdentityTest",
        "version": "1.0"
      }
    }
```
## 2. Starting an Author agent with semi-automatic endorsement (and post-deployment settings)
We're going to start the Author agent with no invitation previously created by the Endorser Agent. 
With this setting the Author agent would be able to find by-itself the endorser's connection id, so you dont have to specify the variable `endorser_connection_id` while creating the payload of the transaction.

### Endorser part 1

We are going to use the same `endorser-auto.yml` but we are not going to use the invitation created by default while setting the Author agent.
1.  Set the config file ```endorser-auto.yml``` in the ```docker-compose.yml``` file like this ```--arg-file './configs/endorser-auto.yml'```
2.  Start the endorser with ```docker compose up ```
3.  Endorser agent is running

### Author part 1
Similar to the automatic process but we are not going to use the invitation as a start parameter in ACA-Py Author agent. So you can comment the `endorser-invitation` parameter in the yaml file (`author-auto.yml`) before starting the agent.
Make sure the following parameters are set correctly (***Replace the endorser-public-did value with your Endorser public DID*** ).
    
  ``` yml 
    endorser-protocol-role: author
    endorser-public-did: R1SbxVKLcBRSHj9s55MAGf
    endorser-alias: Accréditeur   
    auto-request-endorsement: true
    auto-write-transactions: true
    auto-create-revocation-transactions: true
  ```
  > Pay attention to the parameter `endorser-alias`. The value of  this parameter must be used while the Author agent accept "manually" the endorser's invitation at the step 8

4.  Set the config file ```author-auto.yml``` in the ```docker-compose-author.yml``` file like this ```--arg-file './configs/author-auto.yml'```
5.  Start the Author agent ```docker compose -f docker-compose-author.yml up``` 
6.  Author agent is running

### Endorser part 2
7.  create an invitation (with auto_accept = true)
`POST /out-of-band/create-invitation`

### Author part 2
8.   Accepts the invitation (with auto_accept = true and alias=The_same_as_endorser-alias_parameter) created by Endorser
`POST /out-of-band/receive-invitation`

9.  `POST /transactions/{conn_id}/set-endorser-role` 
    Parameters:
      - conn_id= Id de la connection avec endorser
      - transaction_my_job = TRANSACTION_AUTHOR 

10.  `POST /transactions/{conn_id}/set-endorser-info`
    Parameters:
      - conn_id= Id de la connection avec endorser
      - endorser_did = The_same_as_endorser-public-did_parameter
    

11. And that's all. For testing we can try publishing the following schema  to the ledger :
 Call the ```POST /anoncreds/schema``` with this body object:

```json
    {
       "schema": {
        "attrNames": [
          "name","last_name"
        ],
        "issuerId": "put-your-did-here",
        "name": "IdentityTest",
        "version": "1.1"
      }
    }
```


