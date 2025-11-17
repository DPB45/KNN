🔑 Create a GitHub Personal Access Token (PAT)

Jenkins should use a Personal Access Token (PAT) instead of your password for security reasons.



#### **Go to your GitHub profile settings.**

#### 

#### **Navigate to Developer settings → Personal access tokens → Tokens (classic) (or Fine-grained tokens for newer setups).**

#### 

#### **Click Generate new token.**

#### 

#### **Give the token a descriptive Note (e.g., "Jenkins Integration Token").**

#### 

#### **Set an Expiration date (or select "No expiration" if necessary, but this is less secure).**

#### 

#### **Select the scopes/permissions required. At a minimum, for reading repositories, you'll need the repo scope (or specific read permissions for Fine-grained tokens).**

#### 

#### **Click Generate token.**



Important: Copy the generated token immediately. You will not be able to see it again.



🔒 Add Credentials to Jenkins

Now you'll add the GitHub PAT to Jenkins' credential store.



Log in to your Jenkins dashboard.



##### **Go to Manage Jenkins → Manage Credentials (or Security → Manage Credentials in some older versions).**

##### 

##### **Click on the (global) domain link (under Stores scoped to Jenkins).**

##### 

##### **Click Add Credentials in the sidebar.**

##### 

##### **⚙️ Configure the Credential Details**

##### **Field	Value/Action**

##### **Kind	Select Username with password.**

##### **Scope	Usually Global (recommended).**

##### **Username	Your GitHub Username (e.g., janedoe).**

##### **Password	The Personal Access Token (PAT) you copied from GitHub.**

##### **ID	A unique, machine-readable ID (e.g., github-pat-for-jenkins).**

##### **Description	A human-readable description (e.g., Jane's PAT for GitHub access).**

##### 

##### **Export to Sheets**

##### 

##### **Click Create.**

##### 

##### **🛠️ Use the Credentials in a Jenkins Job**

##### **Once the credentials are saved, you can use them in your pipeline or job configuration.**

##### 

##### **A. For Freestyle Jobs**

##### **Open your Jenkins job configuration.**

##### 

##### **In the Source Code Management section, select Git.**

##### 

##### **Enter the Repository URL (e.g., https://github.com/your-org/your-repo.git).**

##### 

##### **Under Credentials, click the dropdown and select the ID/Description of the credential you just created (e.g., Jane's PAT for GitHub access).**



B. For Pipeline (Declarative or Scripted)

In your Jenkinsfile, you wrap the steps that need access to GitHub (like checkout or git) within the withCredentials block, referencing the ID you set for the credential.



Groovy



pipeline {

&nbsp;   agent any

&nbsp;   stages {

&nbsp;       stage('Checkout') {

&nbsp;           steps {

&nbsp;               // Use the ID you assigned to the credentials in the Jenkins store

&nbsp;               withCredentials(\[usernamePassword(

&nbsp;                   credentialsId: 'github-pat-for-jenkins', 

&nbsp;                   passwordVariable: 'GH\_PASSWORD', 

&nbsp;                   usernameVariable: 'GH\_USERNAME'

&nbsp;               )]) {

&nbsp;                   sh 'git config --global user.email "jenkins@example.com"'

&nbsp;                   sh 'git config --global user.name "Jenkins Build"'

&nbsp;                   

&nbsp;                   // The password variable holds the PAT

&nbsp;                   checkout(\[

&nbsp;                       $class: 'GitSCM', 

&nbsp;                       branches: \[\[name: '\*/main']], 

&nbsp;                       userRemoteConfigs: \[\[

&nbsp;                           url: 'https://github.com/your-org/your-repo.git',

&nbsp;                           credentialsId: 'github-pat-for-jenkins' // This is the simplest way

&nbsp;                       ]]

&nbsp;                   ])

&nbsp;               }

&nbsp;           }

&nbsp;       }

&nbsp;   }

}

This setup ensures your Jenkins job can securely authenticate with GitHub.



Would you like to search for the specific GitHub scopes required for a different scenario, like posting Build Statuses?

