## Deployment on Ubuntu

### Install dependencies

Install Nodejs
````shell
sudo apt install npm
sudo npm install -g n
sudo n v12.16.3
sudo apt-get update
sudo apt-get  install  build-essential
````

Install Gitbook
````shell
sudo npm install gitbook-cli -g
sudo gitbook -V
````

### Build From Source
````
gitbook build
````
### Setup for Nginx
````
server {
listen       80;
server_name  dev-docs.poly.network;
access_log  /data/install/nginx/logs/dev-docs/access.log  main;

        location / {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            try_files $uri $uri/ /index.html;
            root   /data/install/dev-docs/_book;
            index  index.html index.htm;
        }

    }

````

## OutLine

### Introduction
* [About Poly Network](introduction.md)
* [Guidelines for Reading](guideline.md)

### Development
* [Connect New Chains](new_chain/readme.md)
  * [Develop for Poly Chain](new_chain/relay_chain/relay_chain_development.md)
  * [Develop for New Chain](new_chain/side_chain/contracts.md)
  * [Develop for Relayer](new_chain/relayer/relayer.md)
  * [Test and Launch](new_chain/launch_and_test/launch.md)
* [Build New Bridges](new_product/integrate_bridge/readme.md)
  * [API](new_product/integrate_bridge/bridge.md)
* [Import New Assets](new_product/integrate_assets/readme.md)
  * [Import Token](new_product/integrate_assets/token.md)
  * [Import NFT](new_product/integrate_assets/nft.md)
* [Customize Business Logic Contracts](new_product/integrate_contracts/readme.md)
  * [Guidelines for Developing](new_product/integrate_contracts/Customizing_Business_Logic_Contract.md)
  * [Test and Launch](new_product/integrate_contracts/launch.md)

----
### User Manuals
* [Token Transaction](Core_Smart_Contract/User_Manuals/Token_Transaction.md)
* [NFT Transaction](Core_Smart_Contract/User_Manuals/NFT_Transaction.md)
* [Transaction Acceleration](Core_Smart_Contract/User_Manuals/Transaction_Acceleration.md)
----
### Appendix
* [Contracts]()
  * [CCD](Core_Smart_Contract/Contract/CCD.md)
  * [CCM](Core_Smart_Contract/Contract/CCM.md)
  * [CCMP](Core_Smart_Contract/Contract/CCMP.md)
  * [LockProxy](Core_Smart_Contract/Contract/LockProxy.md)
  * [NFTLockProxy](Core_Smart_Contract/Contract/NFTLockProxy.md)
* [Nodes](Core_Smart_Contract/Nodes/Nodes.md)
* [Chain ID](Core_Smart_Contract/Chain_ID/Chain_ID.md)
* ### Glossary
* [Glossary](GLOSSARY.md)

### FAQ
* [FAQ](FAQ/template.md)
