---
layout: post
title: AWS Amplify Serverless With React
date: 2019-09-23 00:00:00 +0300
description: Basic authenticator example using AWS Amplify tools and React. # Add post description (optional)
img: aws-amplify.jpg # Add image post (optional)
tags: [AWS, React, Software, AmazonCognito] # add tag
---

Very recently I came across the concept of "serverless" so I decided to deep dive and find out more about it. First of all, the concept means exactly what it says - no servers. Well, not really..there are servers somewhere, but the best part is that you can build and run applications and services without worrying too much about servers configuration. The machine resources are allocated on-demand basis by the cloud provider. Serverless is a cloud computing execution model. I decided to put this theory into practice and leveraged the capabilities offered by AWS Amplify.

>AWS Amplify is a set of purpose-built tools and features that lets frontend web and mobile developers quickly and easily build full-stack applications on AWS, with the flexibility to leverage the breadth of AWS services as your use cases evolve. [Source][aws-amplify]

Basically, AWS Amplify does all the leg work for you: from building, shipping to scaling and managing an application. 

### Build

Any application can be connected to the AWS Amplify stack using Amplify CLI or Amplify Studio. On top of this, you can choose from a set of pre-built web components that can be easily customizable. One thing AWS is planning to do in the future is to allow developers to even import designs from Figma that would further get converted into real code.

### Ship

In terms of shipment, connecting the source code found on the GitHub repository is by far one of the best features as the configs are regenerated automatically which makes the deployment of the app much easier.

### Scale and manage

It becomes easier to support multiple environments. You can create as many as you want, even one per feature branch. In this way, testing the changes before pushing to production becomes a piece of cake. Furthermore, these environments can be easily managed outside the AWS console via role creation.

### Amazon Cognito

[Amplify Auth][amazon-auth] allows setting up a secure authentication with a fully-managed user directory. This tool gives control in what users can see or do on the applications due to Amplify Auth's built-in capabilities as well as integration with [IAM][iam] (the access management service in AWS). Nonetheless, users can sign up via personal email or even social media accounts and can opt in for multi-factor authentication.

There is an [ongoing debate][forum] on using [Auth0][auth0] over Cognito as some developers insist that Auth0 is easier to implement and has better docs, while Cognito is still at the inception phase getting traction slowly. However, Cognito seems to be gaining many points at the pricing category as it turns out to be cheaper than Auth0. 

In my example, instead of using the pre-built UI components provided by AWS Amplify, I created a React component from scratch using [semantic-ui-react library][semantic-ui-react]. Amplify automatically handles the refreshing login tokens and signing AWS service requests with short-term credentials. 

The aws-amplify library is very intuitive, the sign-up function only took a few lines of code:

{% highlight ruby %}
import {Auth} from 'aws-amplify'
signUp = async () => {
        const {username, password, email, phone_number } = this.state
        try {
            await Auth.signUp({ username, password, attributes: {email, phone_number}})
            console.log('successfully signed up!')
            this.setState({step: 1})
        } catch (err) {
            console.log('error signing up: ', err)
            if (err.code === "UsernameExistsException") {
                this.setState({existingUser: true, errMessage: err.message})
            }
        }
    }
{% endhighlight %}

### Conclusion

Authentication is just an example of the multiple tools provided by AWS Amplify. Other examples of features are: 
- Datastore (store engine that syncs up all data between mobile/web apps using AWS AppSync)
- Analytics (helps with tracking user interactions and sessions, web page metrics as well as get real-time stream data to better analyze customer insights using Amazon Pinpoint and Amazon Kinesis)
- API (for HTTP requests to GraphQL/REST endpoints to access, manipulate data from Amazon DynamoDB or Amazon Aurora Serverless)
- Functions (allows adding a Lambda function to the project alongside a REST API or as a datasource in a GraphQL API)
- PubSub (for passing messages between app instances and app's backend by creating real-time interactive experiences; it also provides connectivity with cloud-based message-oriented middlewares)

I highly recommend [this introductory tutorial][tutorial] that has step-by-step instructions on how to build your first React application. 

### Github Repo

[React Example with AWS Amplify and Amazon Cognito][github-react-example-101]


[aws-amplify]: https://aws.amazon.com/amplify/
[amazon-auth]: https://aws.amazon.com/amplify/authentication/?nc=sn&loc=3&dn=1
[iam]: https://aws.amazon.com/iam/
[forum]: https://forum.serverless.com/t/auth0-vs-cognito/3160
[auth0]: https://auth0.com/
[semantic-ui-react]: https://react.semantic-ui.com/
[tutorial]: https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/
[github-react-example-101]: https://github.com/andreeaionescu/react-example-101