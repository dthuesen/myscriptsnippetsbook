# NVM - `nvm use` automation

For using different node versions it is good practice to work with nvm. But changing the project needs to keep in mind to run the command **`nvm use x.xx.xx`** to use the right version for the current project. So there are two drawbacks: First one has to remember/to know which version of no has to run for the actual project and the second is, forgetting to run it leads to problems. There are two steps to get it better done. 

#### 1. Setting up a short cut for running the right version of node

1. ```bash
   cd <project folder>
   touch .nvmrc # creates the file
   echo \6.10.3 >> .nvmrc # appends the version to the file
   cat .nvmrc # prints the file's contents
   ```

   This writes a new file **`.nvmrc`** into the project folder, appends the desired version to the file to it and prints-out the file in the terminal.  
  
   With the command **`nvm use`** in that folder nvm will check for the version in the .nvmrc file and use it. NVM will then when command run print a message like this:  
   **`Found '/Users/<folder>/<folder>/<project folder>/.nvmrc' with version <6.10.3>  
    Now using node v6.10.3 (npm v3.10.10)`**

#### 2. Make nvm read the .nvmrc file and check version when changing directories

For that it is required to make a little editing to the **`.bash_profile`**. With this modification nvm automatically reads the **`.nvmrc`** from the directory while changing between folders.

In **`.batch_profile`** add this to the bottom of the file:

```
cd () { builtin cd "$@" && chNodeVersion; }
pushd () { builtin pushd "$@" && chNodeVersion; }
popd () { builtin popd "$@" && chNodeVersion; }
chNodeVersion() {
# if there's a file named ".nvmrc"...
if [ -f ".nvmrc" ] ; then
# use it
nvm use;
fi
}
chNodeVersion;
```

Now with every change to another directory the script will check if there's a **`.nvmrc`** file present and if so it uses the version contained in that file.



