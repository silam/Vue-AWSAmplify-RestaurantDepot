# vue-amplify-res

This project is about Maintaining the list of Vietnamese Restaurant in MN.
It uses VueJS as FrontEnd, json-server as a test server and AWS Amplify as to configure web backend, to deploy the web app, and to integrate UI components
It also uses DynamoDB as serverless Database to keep the list of Restaurant.

You can see the web running at 
http://vueamplifyres-20210912080612-hostingbucket-dev.s3-website-us-east-1.amazonaws.com/

# Main Page after login

![FrontEnd UI/UX](https://github.com/silam/Vue-AWSAmplify-RestaurantDepot/blob/main/public/MainPage.png?raw=true)


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

- Run "amplify add api" or "amplify update api" if you already one

C:\DEVELOPMENT\VueAmplify\Vue-AWSAmplify-RestaurantDepot>amplify update api
? Please select from one of the below mentioned services: GraphQL
? Select from the options below Walkthrough all configurations
? Choose the default authorization type for the API API key
? Enter a description for the API key: APIRestaurant
? After how many days from now the API key should expire (1-365): 365
? Do you want to configure advanced settings for the GraphQL API No, I am done.

The following types do not have '@auth' enabled. Consider using @auth with @model
         - Restaurant
Learn more about @auth here: https://docs.amplify.aws/cli/graphql-transformer/auth


GraphQL schema compiled successfully.

Edit your schema at C:\DEVELOPMENT\VueAmplify\Vue-AWSAmplify-RestaurantDepot\amplify\backend\api\RestaurantAPI\schema.graphql or place .graphql files in a directory at C:\DEVELOPMENT\VueAmplify\Vue-AWSAmplify-RestaurantDepot\amplify\backend\api\RestaurantAPI\schema
Successfully updated resource



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


### Link to my AWS Account appSync
https://console.aws.amazon.com/appsync/home?region=us-east-1#/niep36rugrenrhj5fusx3m2lce/v1/schema


