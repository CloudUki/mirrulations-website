# Mirrulations Website Repo
**Authors:** Seth Coleman, Davin Glynn, Trevor Gray, Nathan Hajel, Terrell Nelson, Giovanny Teran, and Brandon Yang

This repository contains the source code for the Mirrulations Project website. The Mirrulations Project aims to provide a user-friendly interface for searching and retrieving data from [regulations.gov](https://www.regulations.gov/). This website interacts with an AWS API Gateway to fetch and display data based on user input.

### <ins>**How to Set up the Local Development Environment**<ins>
1. Download relevant repos:
```
git clone https://github.com/mirrulations/mirrulations-website.git
```
2. Inside the _**package.json**_ file put:
```
{
  "name": "mirrulations-website",
  "version": "1.0.0",
  "description": "Website repo for Mirrulations",
  "main": "app.js",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "serve": "vite preview"
  },
  "dependencies": {
    "vite": "^6.1.0"
  }
}
```
3. Install the prerequisites npm libraries
```
npm install dotenv vite react react-dom react-router-dom bootstrap @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons @fortawesome/react-fontawesome
npm install -D @vitejs/plugin-react
```
4. Make a _**.env**_ file:
    - Right click in your root directory.
    - Make a new file, name it `.env`.
    - Inside put `VITE_GATEWAY_API_URL=(Your API GATEWAY LINK HERE)`.
    - Your Gateway URL is the output from when you launch it in the API repo, it might look something like “http://127.0.0.1:3000/dummy” or “http://localost:3000/dummy”     
    - Save the file and run this command:
```
npm run dev
```

### <ins>**How To Get API Gateway Link Running On Amplify:**<ins>
1. Log on to AWS and go to the [AWS Amplify console](https://us-east-1.console.aws.amazon.com/amplify/apps).
2. Click on your app.
3. On the sidebar on the left, click the down arrow on the right side on _**Hosting**_.
4. Click on _**Environment variables**_.
5. On the top right, there should be a button that says, _**Manage Variables**_. Click it.
6. If there is already another environment variable, click _**+ Add new**_ at the bottom at the page.
7. In the box underneath Variable, put `GATEWAY_API_URL`.
8. Then, in the box underneath Value, put your `AWS API Gateway Link`.
9. Click _**Save**_.
10. Go back to your deployment and redeploy your application.
    - In the top left, next to the amplify logo, click _**All apps**_.
    - Click on your app.
    - Click on the branch that you want to redeploy.
    - In the top right corner, click _**↺ Redeploy this version**_.

### <ins>**Start Up AWS Amplify For Production:**</ins>
1. On [AWS](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1#) search for _**AWS Amplify**_.
2. When you select AWS Amplify, you will be brought to the [main page](https://us-east-1.console.aws.amazon.com/amplify). Click on _**Deploy an app**_.
3. Under _**Deploy your app**_, select _**GitHub**_.
    - Click _**Next**_.
4. There will be a window that pops up where you have to Authorize GitHub to work with Amplify. _**Allow it**_, and then you should be brought to the next screen.
5. In the _**Select a repository**_ box, find your repository. If you cannot find your repository, click on the _**Update GitHub permissions**_. If you are working in a different GitHub Organization you will have to allow Authorization to that Organization.
6. You will be brought to App Settings. Hit _**Next**_.
7. Check to make sure that everything is there, and hit _**Save and Deploy**_. It will start to create and deploy your Amplify App.
8. Once the domain is [registered](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar-tld-list.html) through Route 53, go to the Amplify app, and select _**Hosting > Custom Domains**_. Then select the _**Add domain**_ in the top right corner.
9. The domain you are trying to deploy if it has been registered should autopopulate as an option, select the domain, and Configure the domain.
10. No changes need to be made, add domain, and wait. This process can take 5-15 minutes to complete. Now the domain is ready to be accessed. 

### <ins>**Start Up AWS Amplify For Dev Testing**</ins>
1. Head on over to Amplify on [AWS](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1#) search for _**AWS Amplify**_.
2. When you select AWS Amplify, you will be brought to all of your Amplify Apps (if you made the main mirrulations App).
3. At the top right, click on “Create new app”.
4. Select GitHub and authorize Amplify and GitHub if you haven't already.
5. Select your Forked Repository and the branch you are working in.
6. From there, create and deploy the app. 
7. Make sure to get the “Environment Variables” and “Build Settings” from the main Amplify App under “Hosting” and put it within the app your just created, and then redeploy if needed.
8. Key notes: 
   - Make sure on GitHub your fork and branch is synced up to date.


### <ins>**How To Install A Vite Environment For Local Use:**</ins>
1. In the terminal, type: `npm install vite`.
2. Create a file named _**vite.config.js**_.
    - Right click on your root directory.
    - Click _**New File**_.
    - Name it `vite.config.js`.
3. Inside your _**vite.config.js**_ file put:
``` 
import { defineConfig, loadEnv } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig(({ mode }) => {
    const env = loadEnv(mode, process.cwd(), 'VITE_');

    return {
        plugins: [react()],
        resolve: {
            alias: {
                '@': path.resolve(__dirname, 'javascript'), // Alias to access the javascript folder easily
                'styles': path.resolve(__dirname, 'styles') // Alias to access the styles folder easily
            }
        },
        server: {
            host: 'localhost',
            port: 5500
        },
        build: {
            outDir: 'dist', // Specifies the output directory for the build
        },
        define: {
            'import.meta.env': {
                ...env,
            },
            global: {},
        },
    };
});

```

### <ins>How To Create a Github Actions Workflow:</ins>

- It is important to note now and for the future that when making changes to github actions you must **push directly to upstream**.  This is typically bad practice but is required for github actions to work.  Need more information? Check out the  [Discord](https://discord.com/channels/1332506599020822620/1333536321515290646/1336078961943380030).
1. Create a directory ./github/workflows in the main repository. 
2. Create a .yml file which will contain the actions you want to complete.  Check out the template [here](https://github.com/mirrulations/CIWebTest/blob/main/.github/workflows/github-actions-demo.yml).
    - These commands are run from an ubuntu terminal by a “runner” created by github for this purpose
3. Create a github secret to store AWS credentials, this is accomplished in the following steps
    - Click _**Settings**_
    - Under _**Secrets and Variables**_ click _**Actions**_
    - Click _**New repository secret**_ and add the secret name and data
        - Remember to create a secret for both the access key and the secret access key

After all this is done your github actions file is complete, you can test it by pushing some minor change or with this [repo](https://github.com/nektos/act) found by our teammate Owen.

### <ins>Figma Student Account:</ins>
Student Free Account:
1. Make sure to make an account prior to this. Make sure you are using your school account. Without a prior account you will not be able to get the Student Free Account. 
2. Head to this site [Figma Education](https://www.figma.com/education/higher-education/)
3. Hit “Get Verified” 
4. Follow the steps that are prompted. 

Download the Desktop App:
1. When you get verified for your account. Under your profile icon, in the drop down menu, there should be a “download desktop app”.
2. Once downloaded, ask for an invitation from the owner (Dr.Coleman).

