# Multi Machine automated deployment
![](images/image1.png)

Basic diagram on the basics of Jenkins
## Creating a Jenkins pipeline
So, what is the point of a pipeline:

![](images/image2.png)

It lets you run different items based on the outcomes of other items see! So let's make one:

1. Click ![](images/new-package.png)New Item
2. Give it a name and click ![](images/freestyleproject.png)Freestyle project
3. Go down and add some build triggers, just make sure it can pass
4. Generate some keys, add the public key to the repository you want jenkins to access and add the private key to jenkins itself. A reminder of how to do that in [this repo](https://github.com/monotiller/engineering89_git_github#generating-ssh-keys)
5. Go to Post-Build Actions and type in the name of a project you'd like to run after this build has completed. You also have the option to trigger if build is stable, unstable or if it has failed
6. Build your project and check the console to see what it has done!

## HELLO DEV BRANCH
a.k.a. testing and merging in Jenkins

For this we are going to need two jenkins items, one for running the test on our dev branch and one for merging the dev branch in to main if the tests pass

### Testing item
So, let's create our test item:

1. Click ![](images/new-package.png)New Item
2. Give it a name and click ![](images/freestyleproject.png)Freestyle project
3. Tick GitHub project and put in the HTTPS link to your repo
4. Check "Restrict where this project can be run" and enter in your Label Expression
5. Generate some keys, add the public key to the repository you want jenkins to access and add the private key to jenkins itself. A reminder of how to do that in [this repo](https://github.com/monotiller/engineering89_git_github#generating-ssh-keys)
6. Then in Source Code Management select "Git" and enter in your SSH repo url and add your private key
7. In your branches to build add in `*/main` and `*/dev` as your branches to watch
8. In "Build Triggers" check "GitHub hook trigger for GITScm polling"
9. In "Build Environment" check "Provide Node & npm bin/ folder to PATH" and fill in respectively
10. Next fill out your "Build" with some shell commands:
    ```bash
    cd app
    npm install
    npm test
    ```
11. In "Post Build Actions" you are going to want to link it to your merging item (which we will create next). If you already know what you want to call it post it in here, or else just come back later

### Merging item
1. Click ![](images/new-package.png)New Item
2. Give it a name and click ![](images/freestyleproject.png)Freestyle project
3. Tick GitHub project and put in the HTTPS link to your repo
4. Check "Restrict where this project can be run" and enter in your Label Expression
5. Generate some keys, add the public key to the repository you want jenkins to access and add the private key to jenkins itself. A reminder of how to do that in [this repo](https://github.com/monotiller/engineering89_git_github#generating-ssh-keys)
6. Then in Source Code Management select "Git" and enter in your SSH repo url and add your private key
7. In additional behaviours add a merge before build and merge origin in to main 
8. Some shell commands to merge the branches
    ```bash
    git checkout main
    git merge origin/dev
    ```
9. In "Post Build Actions" setup a "Git Publisher" and set it to "Push Only If Build Succeeds" and branches you want to push `main` to `origin`