---
title: "Improve Gitlab CI Build Time on React App"
date: 2021-03-04T11:15:52+08:00
draft: false
comments: true
---

Recently at my work, we have a work building internal dashboard and my colleague (a Front End Engineer) has picked React App for the front end. I got a job as an accident dev ops (it because our office does not have a dedicated dev ops yet).

My job is uploading local project into some accessible url for our user. First, i just ask the front end to send me the project that i have to upload to the server. Problem solves at first.

### a Problem arise
Day later, i realize it's not efficient work. i have to ask regularly to the front end if any changes to the project. so i have to upload it again and again.

After some research, i decided to build Continuous Integration for the project. Btw, our project is hosted in gitlab as our repository management and Gitlab has a nice tools called Gitlab CI for building some automatic deployment.

at first, i got some problem with the Gitlab Runner. it sometimes doesn't run or i have to reinstall it. and it solved finally.

For this project, i just set up 2 stages, build and deploy stages. we have not implemented testing stage for the project yet.

after some trial and errors, finally it has succesfully build and deploy to the server.

### Slow Build Time
for the first process, i was impressed for the result, i don't have to ask the front end over and over again.

but, after some deployment, it become slow deployment on the gitlab ci.

![Slow Build Time Gitlab Ci With Yarn](/img/gitlab2.png)

if you have not concerned with the time, it's oke. but i think it will become a problem if deployment takes more time. 

#### Cache Packages
After searching on the internet, i found that the solutions is really simple. just cache everything. The pain poin of the deployment is build stages takes more than 10 minutes until it done. everytime build stages running, it will download all packages that depend on the project. So i cache it.

`cache:
    paths:
      - node_modules/
      - .yarn
  before_script:
    - yarn install --cache-folder .yarn`

it simply cache node_modules into cache folder.

### Result
![result](/img/gitlab.png)

as you can see, the deployment time has increased significantly from 10 minutes more to just 4 minutes. Thanks to the cache and also internet. :D