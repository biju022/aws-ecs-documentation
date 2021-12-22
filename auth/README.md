# AUTH - PARA Docs

Use the same configs as of https://github.com/Virtusa-Systems-LLC/vcp-auth this repo

You can use the task-defintion.json to create ECS task and run it

# Post Deployment steps

IMPORTANT NOTE - Access Key and Secret Key are only shown once. Make sure you copy it properly for future use

STEP 1 - Access the Auth task's IP or URL via browser and add path /v1/_setup after you URL/IP to get one time Access Key and Secret Key

STEP 2 - Now go to Para Web console - https://console.paraio.org/hello.html

STEP 3 - Use the setting icon at top right to show advanced options and fill out all the fields

        Access Key - Paste you key here
        Secret Key - Paste you key here
        Para API Endpoint - Your task/URL for para
        API Path - /v1/

STEP 4 - Use connect button to login to your para web console

For further information refer the official docs of para here - https://paraio.org/docs/

Docker Hub link - https://hub.docker.com/r/erudikaltd/para

Github Link - https://github.com/Erudika/para