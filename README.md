<h1 align="center">
    <img src="https://github.com/hash3liZer/profileo/assets/29171692/013b6c89-e5af-4fc6-b4b9-f2b6791f2092" alt="Profileo" /> <br>    
    Profileo 🫠
</h1>
<h4 align="center">The Portfolio that explains the what, who, and how of the developer inside me!</h4>

This repository is based on [DeveloperFolio](https://github.com/saadpasta/developerFolio/issues) by [SaadPasta](https://github.com/saadpasta) from Karachi, Pakistan. So, all the development credit goes to him. Guy literally made this. KUDOS!

For the complete guide regarding development and changes, you can follow the original guide. As that one is much more detailed. However, i'll be referring to the final deployment stages as many things including the libraries and workflows were outdated in the original one. And i'd to work my way through the deployment stage. 

# Installation
## Getting Started
You can go for the docker build as well as mentioned in the original document. But, i'll be doing it from scratch just for the known issues with the outdated libraries.

The environment tested and deployed on:
  * Node 18.16.0
  * NPM 
  * Yarn
  * Ubuntu >=20.04 
  * Github Pages for deployement (Multi-Repos)

The first step is to install node and the required packages. To install `node`, use `nvm`. Download it, give permissions and install it:
```bash
$ curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh -o install_nvm.sh
$ chmod +x ./install_nvm.sh
$ ./install_nvm.sh
```

It will return you something like this: 
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Add the above lines to your `~/.profile` and either `source` the profile file or simply exit and enter your shell again:
```
$ source ~/.profile
```

After doing so, you will be able to access `nvm` on command line now: 
```
$ nvm ls-remote
```

![image](https://github.com/hash3liZer/profileo/assets/29171692/b82be75c-2075-4d16-a723-df019f6d67d5)

Next, install the node.js version and tell nvm that you want to use that version of node.js:

```
$ nvm install 18.16.0
$ nvm use 18.16.0
$ node --version
```

Additionally, you can verify the version of NPM as well: 
```
$ npm --version
```

Next, install `yarn` for installing the libraries in the project. 
```
$ npm install yarn --global
```

Now, clone the repo and install the required liraries: 
```
$ git clone https://github.com/hash3liZer/profileo.git
$ cd profileo
$ yarn install
```

Now, you need to setup the `.env` file. Create a `token` in your github `developer` settings and paste it in the file where required along with your github username: 
```
$ cp .env.example .env
$ nano .env               ## Edit the file here
```

And run the project in development mode:
```
$ npm start
```

## Changes

Just change `src/portfolio.js` to get your personal portfolio. Customize portfolio theme by using your own color scheme globally in the  `src/_globalColor.scss` file. Feel free to use it as-is or personalize it as much as you want.

For more changes, please follow the original guide from `saadpasta`. Its much more detailed. 

# Deployment
Now, the deployment steps. Note that, this guide only covers about deployment to Github Pages in detail. For other platform, refer to cocerning guide

## Github Pages & Actions Combined
CI/CD is what we are going to implement next. In other words, this would enable such an environment that if you push a change to the github, the website will automatically be redeployed. (Cool,right?) Lets see, how it works. 
This repo is deployed at `hash3liZer.github.io/` root directory (using Github Actions to push to the `hashlizer.github.io` repository). First, edit to the `packages.json` file and make sure to have the `gh-pages` script: 

```
"scripts": {
    "predeploy": "npm run build",
    "start": "node fetch.js && react-scripts start",
    "build": "node fetch.js && react-scripts build",
    "deploy": "gh-pages -b main -d build",
    
    ...
```

Inside the `deploy` script command note the `-b` argument. This is the branch we will be building the final pages from. So, make sure that this is correct. In our case this is `main` which is the default branch, so we will let it be. 

Next, come to the file: `.github/workflows/deploy.yml` and see the following sections: 
```
env:
  CI: false
  GITHUB_USERNAME: ${{ github.repository_owner }}
  REACT_APP_GITHUB_TOKEN: ${{ secrets.PROFILEO_TOKEN }} # This is automatically set by Github Actions
  USE_GITHUB_DATA: "true"
  #MEDIUM_USERNAME: "hash3liZer"
```

In this section, we can the see the env file will automatically be created with the deployment process we need to create some secrets in the repo which we will see in a while. Now, in the last section see: 
```
     - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          token : ${{ secrets.PROFILEO_TOKEN }}
          branch: gh-pages 
          folder: build
          repository-name: 'hash3liZer/hash3liZer.github.io'
```

Here, we are using a workflow from Github Marketplace for deploying our project using github pages. You can refer to its complete documentation here: https://github.com/JamesIves/github-pages-deploy-action
However, the important thing to note here are the options we are using. The `branch` specifies the target branch where the production files from the project will be placed. In our case, we told it to be: `gh-pages`. The `folder` argument tells the name of folder where the production files that we got from npm are placed. And finally, the `repository-name` specifies if you want to deploy this application to another repo which is also being done in this case. You can skip this option if you are deploying in the same repo. 

After doing this, we need to add the secret variables. First, go the `Github -> Settings -> Developer Settings -> Create Fine Grained Token`. Note that this token generated should have the write access to the necessary repos. Otherwise, you are going to see `Permission` errors in your workflows. Add the Secret variable in the repo settings:

![image](https://github.com/hash3liZer/profileo/assets/29171692/55a5306a-a4b1-46b0-aa75-27e0782601d1)

And thats it, now push the done changes to your repo and you will see Github Actions on the run. 

![image](https://github.com/hash3liZer/profileo/assets/29171692/5931a01e-73a5-4be4-8310-2e50ea637780)

Now, we just need to enable `Github Pages` for the website to be live for the public. Go to the github settings -> **Pages**. If you deployed on the same repo, simply select `Github Actions` and if you deployed on a remote repo, then select `Github Branch` option. And select the `gh-pages` branch as specified in the workflow. If everything works fine, you will now able to see your website live at `username.github.io/..` in a couple minutes after the workflow is complete.

## Get me At

<a href="https://shameerkashif.me"><img src="https://avatars.githubusercontent.com/u/29171692?s=400&u=ab1e566749c83b4a9ffe1cefbe55857186139978&v=4" width="100px;" alt=""/><br /><sub><b>Shameer Kashif</b></sub></a>


---
