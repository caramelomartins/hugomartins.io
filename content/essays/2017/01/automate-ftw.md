---
title: "Automate Simple Deploy Workflows - Cool!"
draft: false
date: "2017-01-08T23:44:20Z"
aliases: ["/automate-simple-deploy-workflows---cool/"]
categories:
    - bash
---

I recently changed my blog from [Jekyll](https://jekyllrb.com/) to [Hugo](https://gohugo.io/) because it's fast, has custom support theme, **shares my name** and I never got along with Jekyll, honestly.

With changing my configuration I had to make some changes to the workflow I used with my blog and writing. With that in mind I created a new repository - [website](https://github.com/caramelomartins/website) - which holds the actual source code that Hugo takes to build the website. The website is then hosted with Github Pages with a custom theme at [hugomartins.io](https://hugomartins.io).

With this new setup in mind I got tired really fast of deploying the compiled code from one repository to another and because I'm lazy I decided to automate the deployiment with a simple script called [deploy-sh](https://github.com/caramelomartins/website/blob/master/scripts/deploy.sh).

The script is very simple:

```
#!/bin/bash

echo "Building website..."

cd ..
hugo -t cocoa > /dev/null 2>&1
cd "public/" || exit

echo "Copying new website into other repo..."

cp -R ./* ../../caramelomartins.github.io/

echo "Push changes to remote..."

cd "../../caramelomartins.github.io/" || exit
git add -A > /dev/null 2>&1
git commit -m "Push new content" > /dev/null 2>&1
git push > /dev/null 2>&1

echo "Finished deployment..."
```

The only requirement basically is that the repository must be configured to use SSH otherwise the script won't be able to push to the other repository. Also, both repositories must be inside the same parent folder.

The script simply goes to the source code directory and compiles the website. It then takes the compiled files and copies them over to the other repository. After that, it commits everything with a generic message which I'm hoping to improve and then pushes the data into the repository.

It's a very simple script and I see lots of interesting opportunities of improvement - like registering the commit hash of the deployment in the commit message - but for now it does the work!
