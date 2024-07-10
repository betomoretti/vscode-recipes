# Debugging node processes running in a docker container using vscode

1. Start your container using the node debugger command. For example:
    ```
    FROM node:20
    WORKDIR /usr/src/app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 3000 9229
    CMD ["node", "--inspect=0.0.0.0:9229", "app.js"]
    ```
    And then
    `docker build -t my-node-app . && docker run -p 3000:3000 -p 9229:9229 -v $(pwd):/usr/src/app my-node-app`
2. If you haven't yet, create a `.vscode` folder at your working directory and add a `launch.json` file
3. Place the following settings:
    ```
    {
        "version": "0.2.0",
        "configurations": [
          {
            "name": "Docker: Attach to Node",
            "type": "node",
            "request": "attach",
            "port": 9229, 
            "address": "localhost",
            "localRoot": "${workspaceFolder}",
            "remoteRoot": "/usr/src/app",
          }
        ]
    }
    ```
    **It's important that the remoteRoot maps with the folder holding the files inside the container. Otherwise, the breakpoints won't bound**   
4. Finally, press the red play icon that appears in the Run & Debug section.
