QUIZ GAME

Description:
A simple quiz game that lets users register with an account (email address) to solve questions about cloud and its related services. The answers to the questions are hardcoded and the game is a React App that uses Amazon Cognito, AWS Amplify, CICD and GitHub.

Required services/tools:
- Amazon Cognito
- AWS Amplify
- GitHub repository to store the files/codes
- JavaScript file
- AWS IAM
- Visual Studio Code (VSCode)
- Node.js installed
- Amplify CLI

Key files/code
App.js: The React application that's configured to use Cognito for authentication
Quiz.js: The Quiz component
quizData.js: The hard-coded questions and answers for the quiz


STEP 1 - Set up the environment
- Create a folder on your desktop (or other location) e.g React-App
- Open VS Code editor and select the folder => new terminal => Change directory to the folder.
- Run the following commands
  npm install -g @aws-amplify/cli			   ---> To Install Amplify cli by running the command  
  amplify configure					   ---> To configure Amplify to make the cli work with AWS account. This command will open a page for your AWS account.
- Log in to AWS console with your credentials. Then go back to VS Code editor to continue
- You will need to select your REGION and press ENTER. This will display a link on what to do next (amplify documentations)
- A new page (IAM) for you to create NEW USER => Give name and attach policy directly => AdministratorAccess-Amplify => create user
- Go back to VS Code, press ENTER. It will ask you for accessKeyID and secretKeyID. Navigate to your newly created user and generate the keys.
- Your new user => security credentials => access keys => use case = command line interface => create key(s). Copy the keys and go back to VS Code to continue
- Paste the keys in the editor and continue. This action would update/create the AWS profile in your local machine
  *profile name = amplify-admin-local


STEP 2 - Create the React App
- Run the command below
  npx create-react-app <name of your app> e.g npx create-react-app cloud-quiz-app
- It may ask for permission to install react package, click Yes. You will see "Happy hacking!" at the end of the execution. 
- You will also notice sub-folder opened for the app under your original folder ( i.e  React-App/cloud-quiz-app/dependencies). 
*Node_modules
*src = App.css , App.js , index.css etc
*package.json
*package-lock.json etc
*README.md etc
- change directory to the React app created
  cd cloud-quiz-app
- Run the following commands
  amplify init
- You can use the default project name provided. This is what will show on AWS console. Review the details => Yes (to initialize the project)
- Select AWS profile. It will display the local profile you created earlier (in my case, amplify-admin-local).
- The necessary backend environment will be created/deployed to AWS Amplify.
- You can decide to share non-sensitive info. Here you decide Yes/No...........select No.
- You should see "deployment state saved successfully"
- Navigate to AWS console => Open Amplify => You should see the app on the landing page => View App => You will see deployment status (dev).


STEP 3 - Add authentication with Cognito
- Go back to VS Code editor to add connections between Amplify and Cognito for the authentication.
- Run the command below 
  amplify add auth
- Then select "Default configuration" => Select sign-in method for users => Email => No, I'm done (for advanced settings).
- These actions will successfully add auth resource to the App locally. 
- You can then PUSH to build all your local backend resources and provision it in the cloud.
  amplify push 
- Navigate to AWS console, search and open Cognito. You should see the user pool created through the last command....e.g cloudquizapp3d23dh56_userpool_3d23dh56-dev
- Click on the userpool to see all the details. No user created yet.

Create user
- Go back to the VS Code, click on App.js (this is the main file for the React App) => replace the content with those in the GitHub repo/App.js
- Comment out line 19   .... //import Quiz from "./Quiz'
- Change line 32 from <Quiz /> to {/* <Quiz />*/}
- Install Amplify libraries that give us the login user interface. Run the command below
  npm install aws-amplify @aws-amplify/ui-react		---> To create the user interface
  npm start						---> To run the application
- Upon successful start, a new web browser will open showing the login page. If not, you can view your quiz app (cloud-quiz-app) in the browser using http://localhost:3000 or http://192.168.12.112:3000

Manage the react App to create account and sign-in
- Click "Create Account" => complete the details => You need a valid email for this as it needs to be verified => Create account
*Username
*password
*email 
- A confirmation code will be sent to the email used for the account creation. Put the code in the space provided => confirm = sign out
- It will take you back to the landing page for Sign In or Create Account. You can sign in with the last created account (username and password) => sign out.
- Navigate to Userpool under Cognito => refresh the tab => You will see the created account with status confirmed and email verified.


STEP 4- Add functionality and styling for the Quiz
- Navigate back to VS Code editor, create a new file under "src" and name it Quiz.js
- Copy the content of Quiz.js in the GitHub repo and paste the content in the file you created above.
- Create another file under "src" and name it quizData.js.
- Copy the content of quizData.js in the GitHub repo and paste the content in the file you created above. QuizData.js contains all the questions and answers for the quiz app.
- Click on App.js to undo the actions of Step 3 i.e in line 19, remove // and {/* */} in line 32 (you should have just <Quiz />
- Save the file => Your React App page should refresh with the question and answer option showing. If not, login/sign-in again with the last user credential.
- You can test the game by answering some of the questions. If the answer choice is right, you will see "Correct ". Otherwise, "Sorry, that's not right. "


STEP 5 - Push local code to GitHub repository
- Earlier, the front end was ran locally on localhost:3000 with Cognito as the backend. 
- However, in real world, your code will either be in GitHub (or other SCM like Gitlab, Bitbucket etc). You then hook it up with Amplify and host the front end there.
- The process will be as follows:
* We will push our codes currently in VS Code to a GitHub repo and then set up Amplify to pull from that Repo.
* Amplify will host the Front end and whenever we make a change to the code in GitHub, we want to trigger a new build and deploy Amplify. This creates a CI/CD pipeline set up.

Push the codes to GitHub
- Navigate to GitHub and open your account profile => Create a new Repo to host the codes => copy the URL
- In VS Code, run the following commands
  git init
  git add .
  git commit –m "Initial commit"
  git branch –M main
  git remote add origin <repository URL> e.g git remote add origin https://github.com/thecloudenthusiast/cloud-quiz-app.git
  git push –u origin main


STEP 6 - Host the front-end in Amplify vis GitHub (for CI/CD implementation)
- Navigate to AWS console => Amplify => Your App => Branches (Deploy frontend) => Get started => choose source code provider => GitHub => Next
- A new page will pop up => Authorize => select the Repo => Install & Authorize
- Select the repo from the dropdown => Next => leave or edit the app name => 
*environment = dev
*checkmark "Enable full-stack CI/CD"
- Select a service role (if you have already) or click on "create a new role" => AWS Service => Amplify => Next =>Permission=AdministratorAccess-Amplify => Next => create role
- Go back to Amplify, refresh the role tab => select the new role => Review = Save and deploy => The status will show "deployed" when successful.
- Click on "Visit deployed URL" to see the App. This is a real URL as we no longer need localhost to access the App.
- Sign-in with the credential of the user created earlier.


STEP 7 -Confirm CI/CD set up
- Go to the quizData.js file on GitHub repo, edit the content => this should trigger Amplify to detect the change and automatically restart the process (provision-Build-Deploy).

  P.S: Full credit goes to TinyTechnicalTutorials for the original source codes.

