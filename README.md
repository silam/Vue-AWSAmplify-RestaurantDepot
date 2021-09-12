# vue-amplify-res

This project is about Maintaining the list of Vietnamese Restaurant in MN.
It uses VueJS as FrontEnd, json-server as a test server and AWS Amplify as to configure web backend, to deploy the web app, and to integrate UI components

## Project setup
```
npm install -g npm

```
- Install vue-cli
npm install -g @vue/cli

- Install aws amplify API and vue library 

npm install --save aws-amplify @aws-amplify/ui-vue

- Install AWS Amplify CLI

npm install -g @aws-amplify/cli

- Run amplify configure to enter accesskeyid and secretkeyid

- Run amplify init to configure project name and environment

- Run amplify add auth to use authentication

- Run amplify push to create resources in AWS Account

- Run amplify status to check status of the newly created AWS Cognito User Pool 

- To look at Cognito anytime, go to
https://console.aws.amazon.com/cognito/

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
