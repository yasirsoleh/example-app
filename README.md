# Initializing Laravel Project for Beginners
Below we will create a new Laravel project with Laravel Breeze authentication scaffold. We will also install Daisy UI to provide us with Tailwind CSS components. We also use git and GitHub to manage the codes.
## Prerequisite
One time step only. To prepare the development environment.

### Install
Please download and install prerequisites listed below. For Linux and macOS users,Windows Subsystem for Linux and Windows Terminal is not needed. Instructions that call for usage of Windows Terminal, just use normal terminal emulator.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Laravel Extension Pack for VS Code](https://marketplace.visualstudio.com/items?itemName=onecentlin.laravel-extension-pack)
- [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install)
- [Docker](https://www.docker.com/products/docker-desktop)
- [Beekeeper Studio](https://www.beekeeperstudio.io/)
- [Windows Terminal](https://aka.ms/terminal)

### Prepare for Using Git
Do this thing to make sure you can use git and push your codes to GitHub properly

#### 1. Generate SSH keys
Open a Windows Terminal, the open a new Ubuntu terminal tab. Head to your home directory.
```
cd
```
Use this command to generate ssh key pair.
```
ssh-keygen
```

#### 2. Copy Your Public Key

Use this command to view your public ssh key. And then copy paste the outputs of this command.
```
cat ~/.ssh/id_rsa.pub
```

#### 3. Add Public Key to GitHub

Go to [GiHub settings](https://github.com/settings/keys), navigate to SSH and GPG keys. Click New SSH Key and then paste the content of your public key.

#### 4. Set Git Name and Email

We will set a global name and email to associate with the commit that we will do. Open a terminal and then run these command with replacing with your name and email.

```
git config --global user.name "Example Name"
```
```
git config --global user.email "example@example.com"
```
To verify the changes,
```
git config --list
```

### Setup Bash for Laravel Sail

To avoid using long './vendor/bin/sail' and use just sail command to use Laravel Sail.

If you used macOS, instead of editing ~/.bashrc, please edit ~/.zshrc as macOS use zsh instead of bash.

#### 1. Adding Laravel Sail Bash Alias to .bashrc. 
Open a Ubuntu terminal from Windows Terminal. 
Head to your home directory.
```
cd
```
Then edit .bashrc using nano. 
```
nano ~/.bashrc
```
After this append sail alias to the bottom of the file and save the changes.
```
alias sail="./vendor/bin/sail"
```
#### 2. Use Edited .bashrc
Reload the terminal to use the new .bashrc using command below.
```
source ~/.bashrc
```


## Creating New Project
This is the for first time creating this project, it should not be followed for existing project from the repo.

`example-app` will be used as the name in this example, but you should replace `example-app` with your project name for example `psm`.

### 1. Initilizing Laravel project
Reference: https://laravel.com/docs/8.x/installation

According to Laravel documentation, open a Windows Terminal, open a new Ubuntu terminal tab and then paste this command.

```
curl -s https://laravel.build/example-app | bash
```

This will download the Laravel project template. 

### 2. Run the Laravel Project

Navigate to the new folder created by the above command.

```
cd example-app
```

And then open VS Code to the current directory.

```
code .
```
Open a new Terminal in the VS Code window.
Then run Laravel Sail. This will run the docker container that contains all the required services for Laravel development environment such as PHP, MariaDB, Composer, and Node.js. '-d' option is for detached so that is does not bind our terminal.
```
sail up -d
```
Then everytime you initialize, clone, or make changes to the database for Laravel project you should run migration and database seed
```
sail artisan migrate
```

```
sail artisan db:seed
```

### 3. Manage the Database

Open Beekeeper Studio. Then choose the connection type which is MariaDB. Set the host as 'localhost', port is '3306', User is 'sail' and Password is 'password', connection name will be the name of your project. Click save and then connect.

### 4. Make Your First Git Commit
Create a New Repository in your GitHub. The repository should be same as the name of your project for example in this case, `example-app`. 

**DO NOT ADD README, .gitignore, and the license** yet because the folder initialized with previous command already have it.

At the current project directory in the terminal. Run this command to init git at your project directory.

```
git init
```

Add all the new files to be commited.

```
git add .
```

Commit using this command and the also the '-m' is for the commit message in this case is "first commit"

```
git commit -m "first commit"
```

Move the master branch to main branch to match with GitHub

```
git branch -M main
```

Then add the remote repository which is the repo that you created earlier, in my case is `yasirsoleh/example-app`. Change to URL that GitHub gives you.

```
git remote add origin git@github.com:yasirsoleh/example-app.git
```

Then push the commit to the remote repo.

```
git push -u origin main
```

You will see that your new Laravel project is on the GitHub.

### 5. Installing Laravel Breeze
Reference: https://laravel.com/docs/8.x/starter-kits#laravel-breeze

Laravel Breeze is the scaffold that implement basic authentication in Tailwind CSS and Blade so that serve as the starting point. 

First we need to add Laravel Breeze to our composer.json. This command will also install Tailwind CSS.
```
sail composer require laravel/breeze --dev
```
Then run this command to install from the composer.json.

```
sail artisan breeze:install
```

Then you should download the node depedencies using npm, this command will create the node_modules folder. You should run this command if you changed the package.json folder.

```
sail npm install
```

After that you should run this command to compile the JavaScript and CSS assets. Run this command when you change the styling.

```
sail npm run dev
```

Then run the migration to update the database.

```
sail artisan migrate
```
It is a good idea everytime you changed something, commit the changes so that you can rollback in case of errors. For example my commit message is "adding Laravel Breeze".

```
git add .
```

```
git commit -m "adding Laravel Breeze"
```

```
git push
```

### 5. Installing daisyUI

Reference: https://daisyui.com/docs/install
Install daisyUI using npm

```
sail npm i daisyui
```

Append require(daisyui) to the plugins in tailwind.config.js

```
module.exports = {

    plugins: [
        require('example'),
        require('daisyui'),
    ],

}
```

Then npm run dev to compile the newly added 
```
sail npm run dev
```

It is a good idea everytime you changed something, commit the changes so that you can rollback in case of errors. For example my commit message is "adding daisyUI".
```
git add .
```

```
git commit -m "adding daisyUI"
```

```
git push
```


## From Existing Project

### 1. Clone the repository
```
git clone git@github.com:yasirsoleh/example-app.git
```
### 2. Make sure to be inside the directory
```
cd example-app
```
### 3. Composer install using docker to prepare vendor folder
```
docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v $(pwd):/opt \
    -w /opt \
    laravelsail/php80-composer:latest \
    composer install --ignore-platform-reqs
```
### 4. Prepare .env
```
cp .env.example .env
```
### 5. Edit .env and change the variable as follows
```
DB_HOST=mariadb
DB_USERNAME=sail
DB_PASSWORD=password
```
### 6. Run using sail detached
```
sail up -d
```
### 7. Run node dependencies
```
sail npm install
```
```
sail npm run dev
```
### 8. Run migration
```
sail artisan migrate:fresh --seed
```

## Running and Stopping Laravel Sail

For stopping sail, go to project directory, then sail down.
```
sail down
```

To run again, use sail up again at the project directory.
```
sail up -d
```
