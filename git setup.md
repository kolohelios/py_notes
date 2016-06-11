git config --global user.name "Beyonce Knowles"
git config --global user.email beyonce@thinkful.com

https://github.com/github/gitignore/blob/master/Python.gitignore

Next, you may optionally set up a list of files which you want Git to ignore. 
Make a new file (not a directory) in your home directory (~) called 
.gitignore_global (notice the leading dot). 
Copy the contents of this file into .gitignore_global. Then tell Git to use 
the file as a guide to which files to ignore. Again, this will only need 
doing once. Use the following command:

git config --global core.excludesfile ~/.gitignore_global