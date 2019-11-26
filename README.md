# yubo-yue.github.io
Yubo's own knowledge Base

http://jekyllrb.com/docs/usage/ is about how to deploy and test locally.

```
# install ruby by rbenv
# update brew latest formula
brew update && brew upgrade ruby-build  

# see all ruby available version list
rbenv install --list

# install latest ruby from rbenv
rbenv install 2.6.5

# 


jekyll --version

# list local gem for jekyll 
gem list jekyll

# remote gem for jekyll
gem search jekyll --remote

# update jekyll
gem update jekyll

jekyll server

# with draft support
jekyll server --draft


jekyll build
# => The current folder will be generated into ./_site

jekyll build --destination <destination>
# => The current folder will be generated into <destination>

jekyll build --source <source> --destination <destination>
# => The <source> folder will be generated into <destination>

jekyll build --watch
# => The current folder will be generated into ./_site,
#    watched for changes, and regenerated automatically.
```