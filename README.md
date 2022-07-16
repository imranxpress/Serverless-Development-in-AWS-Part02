# Serverless Application Development : Amplify + Cognito + API Gateway + Aurora 🛠 Part: 02 

![aws01](https://user-images.githubusercontent.com/47071968/179358452-291234d3-2f2b-4192-b714-b4159a74dee2.jpg)

In the last episode I talked about Amplify and we created our first Amplify web application with a automated deployment pipeline.

### 📌Please have a look if you missed it! 📙 📚

 [Serverless Development with AWS Amplify 🛠 Part: 01](https://github.com/imranxpress/Serverless-Development-in-AWS-Part01)

Today I am going to develop backend for our application and all I will be doing with Amplify CLI [Command Line Interface]. This will help us build a CloudFormation Template which can be used later on as an IaC [Infrastructure as Code]. I will talk a lot more about IaC later on another article series. 😃


Let's get started with designing the most important thing called "Solutions Architecture". I love to do this part, may be if I have had the opportunity I could design Solutions Architectures ALL DAY EVERY DAY! 😂 🤣 😂

Obviously to do so we must have to do a lot of Research and Deep Dive into technical services you will be using for the Solution Architecture.

### 📌So here is the Architecture bellow which we will be implementing for our backend which is fully Serverless.

![aws02](https://user-images.githubusercontent.com/47071968/179358878-b939927c-8a14-484f-99da-66359e041c0e.png)

### Let's fire up the CLI...🔥 🔥 🔥

We will use the same IAM user we created on our last article and use it for configuring Amplify CLI.

Make sure you have AWS CLI installed and configured in the same region as your Amplify app is running on. if you are new at AWS CLI then have a look on this link for basic installation.

###  [Installing or updating the latest version of the AWS CLI.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)


Now that you have AWS CLI installed let's install Amplify CLI, ok little bit about Amplify CLI: The Amplify Command Line Interface (CLI) is a unified toolchain to create, integrate, and manage the AWS cloud services for your app.

### Run this bellow command and get the Amplify CLI installed!

~~~
npm install -g @aws-amplify/cli

~~~

### Now run this command to configure Amplify on your local machine!

~~~
amplify configure
~~~

This command will ask you a series of questions and you can either create a new IAM user or use the same IAM.

~~~
Specify the AWS Region

? region:  # Your preferred region where you have Amplify + CodeCommit

Specify the username of the new IAM user:

? user name:  # User name for Amplify IAM user existing or new


Complete the user creation using the AWS console


Enter the access key of the newly created user:

? accessKeyId:  # YOUR_ACCESS_KEY_ID

? secretAccessKey:  # YOUR_SECRET_ACCESS_KEY

This would update/create the AWS Profile in your local machine

? Profile Name:  # (default)



Successfully set up the new user.

~~~

### Have a look on this for better understanding!👇


 ## [Installing & Configuring the AWS Amplify CLI](https://www.youtube.com/watch?v=fWbM5DLh25U&t=18s)

### 📌Now it's time to Initialize new project.

To initialize a new Amplify project, run the following commands from the root directory of your frontend app. 👇

~~~
cd project/root/folder

amplify init

~~~



This command will ask you a series of question based on your frontend framework used:

![aws03](https://user-images.githubusercontent.com/47071968/179360029-07ac19dc-f4b9-4c1f-8dd9-a0bbf586aef3.jpg)

### 📌When you initialize a new Amplify project, a few things happen:


1. It creates a top level directory called amplify that stores your backend definition. During the tutorial you’ll add capabilities such as a REST API and authentication. As you add features, the amplify folder will grow with infrastructure-as-code templates that define your backend stack. Infrastructure-as-code is a best practice way to create a replicable backend stack.

2. It creates a file called aws-exports.js in the src directory that holds all the configuration for the services you create with Amplify. This is how the Amplify client is able to get the necessary information about your backend services.

3. It modifies the .gitignore file, adding some generated files to the ignore list.

A cloud project is created for you in the AWS Amplify Console that can be accessed by running amplify console. The Console provides a list of backend environments, deep links to provisioned resources per Amplify category, status of recent deployments, and instructions on how to promote, clone, pull, and delete backend resources.

### Your Amplify Backend is added now and you can check that via this command:👇

~~~
amplify status

~~~

![aws04](https://user-images.githubusercontent.com/47071968/179360162-8c32ef9b-ea5d-4f7a-89c4-87354cdf9dab.png)


### 📌Install Amplify libraries:

The first step to using Amplify in the client is to install the necessary dependencies:👇
~~~
npm install aws-amplify @aws-amplify/ui-react

~~~

The aws-amplify package is the main library for working with Amplify in your apps. The @aws-amplify/ui-react package includes React specific UI components we’ll use as we build the app.

## 📌Set up frontend:

Next, we need to configure Amplify on the client so that we can use it to interact with our backend services.

Open src/index.js and add the following code below the last import:👇

~~~
import Amplify from "aws-amplify";

import awsExports from "./aws-exports";

Amplify.configure(awsExports);

~~~


And that’s all it takes to configure Amplify. As you add or remove categories and make updates to your backend configuration using the Amplify CLI, the configuration in aws-exports.js will update automatically.


Now that our React app is set up and Amplify is initialized, we’re ready to use another service from the architecture called Amazon Cognito to add User Authentication feature. Amazon Cognito is a robust user directory service that handles user registration, authentication, account recovery & other operations. To learn more about [Amazon Cognito visit here.](https://aws.amazon.com/cognito/)

### Run this command to add Amazon Cognito Authentication feature:👇
~~~
amplify add auth

~~~
![aws05](https://user-images.githubusercontent.com/47071968/179360534-3ba2af55-f58a-4f58-971b-e5366a03937f.jpg)

### Now check your "amplify status" and see the change:
~~~
amplify status

~~~
![aws06](https://user-images.githubusercontent.com/47071968/179360616-dc255e8e-5189-46b1-8e1d-683f5f8172cb.jpg)


Before testing the Authentication service, we will have to push the Amplify changes to Cloud so that it can be accessible. Run the bellow command:
~~~
amplify push

amplify console

~~~


Last command will take you directly to the AWS Console and see the running Amplify application.

Let's get back to code editor and add the frontend code for the user registration and login.

Open src/App.js and make the following changes:

Import the withAuthenticator component:
~~~
import { withAuthenticator } from '@aws-amplify/ui-react'

~~~


Change the default export to be the withAuthenticator wrapping the main component:
~~~
export default withAuthenticator(App)

~~~

Run the app to see the new Authentication flow protecting the app:
~~~
npm start

~~~

Now you should see the app load with an authentication flow allowing users to sign up and sign in.
![aws07](https://user-images.githubusercontent.com/47071968/179360797-9f1e30ac-14c0-4cbb-aacc-c46166c43a64.png)


Create a new user account and get the OTP in your email to complete registration process.


Now Sign in to your newly created account...

![aws08](https://user-images.githubusercontent.com/47071968/179360843-a8cc067d-97f0-4bf5-9f69-105d6e86362c.png)


Here we have complete 2 pieces of puzzle to complete building our new Serverless application.

### 1. Installing and Configuring Amplify CLI . 

### 2. Adding Amazon Cognito for User Authentication feature.




## 👨‍💻 Thanks for your time and happy learning... 🙏
