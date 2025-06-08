## Matrixbird


Matrixbird is email built on top of matrix. This central repo links to various bits of code necessary to run the Matrixbird suite.


> [!WARNING]  
> This project is experimental, and not yet ready.


### Email 

It is possible to run Matrixbird as a pure matrix-only application, but if you want integration with traditional email, you'll need run Postfix/Dovecot and a transport map to pipe incoming emails to the Matrixbird server. Additionally, you'll need to set up a trusted relay for outgoing email. The instructions for setting up an email backend is out of scope of this document. 


### Matrixbird Server

The Matrixbird server is both a bridge between the email backend and matrix, and an appservice that handles federated matrix emails. The code is in [github.com/matrixbird/server](https://github.com/matrixbird/server). Follow the instructions there to get it up and running. 

### Matrixbird Client

There is a reference client implementation of Matribird at [github.com/matrixbird/client](https://github.com/matrixbird/client). This is a standard frontend app that can be deployed to a VPS or various cloud vendors. 

### LICENSE

I have not yet decided on a license.
