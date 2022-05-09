# Auto Branch Protect
<a href="https://opensource.org"><img height="150" align="right" src="https://opensource.org/files/OSIApprovedCropped.png" alt="Open Source Initiative Approved License logo"></a>

[![CodeFactor](https://www.codefactor.io/repository/github/zkoppert/auto-branch-protect/badge?s=c9ed51e74e4a59d7e3a0e766fe56b1237a53d1c4)](https://www.codefactor.io/repository/github/zkoppert/auto-branch-protect)  [![Total alerts](https://img.shields.io/lgtm/alerts/g/zkoppert/Auto-branch-protect.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/zkoppert/Auto-branch-protect/alerts/)

Forked from Zack Koppert's Auto Branch Protect Tool for GitHub's Technical Assessment credits - https://www.codefactor.io/repository/github/zkoppert/auto-branch-protect

Auto branch protect is a simple web service that listens for organization events to know when a repository has been created. When a repository is created, the web service automates the protection of the main branch. It also notifies you with an @mention in an issue within the repository that outlines the protections that were added.

Add Jenkins webhook in GitHub
Web hook is basically a Http Push API. Its a way for an application to provide another application with real time notifications. Whenever a Developer pushes a change in the Auto-branch-protect's Master branch, a build is triggered in Jenkins. Jenkins is installed in Ubuntu instance in AWS Cloud. 

## Usage
- Install the following:
  - [Python 2](https://www.python.org/downloads/)
    - `pip install -r requirements.txt`
  - [Flask](https://flask.palletsprojects.com/en/1.1.x/installation/#installation)
  - [ngrok](https://dashboard.ngrok.com/get-started)
  - [Git](https://git-scm.com/download/mac)
  - [IDLE](https://docs.python.org/3/library/idle.html)
  - [Jenkins](https://www.jenkins.io/download/)
  - [AWS](https://portal.aws.amazon.com/billing/signup#/start/email)



  <img width="534" alt="Screen Shot 2022-05-08 at 11 17 00 PM" src="https://user-images.githubusercontent.com/104956469/167351261-622ebe50-799b-4d4a-b4c2-6b8ce1bc7da0.png">

  
  
- Set GH_TOKEN as an environment variable with a value that corresponds to a GitHub Token (ie. `export GH_TOKEN=208923487234780287128091`)
- Set the user value in app.py
- Start the local web service via `flask run --host=0.0.0.0 &`

- If you are having issues unable to start FLask on Port 5000, its because of Mac-Settings-System Preferences-Sharing-Airplay receiver ON- Turn this off.

<!-- markdownlint-disable -->
- Start the forwarding service via `./ngrok http 5000 &`
- Note the forwarding address (ie. `https://cfe6d829.ngrok.io` in the output of the ngrok application)
<!-- markdownlint-disable -->
- Set up a WebHook in the desired GitHub organization [example](https://github.com/buzzmoto-org/REPO/settings/hooks)
  - Note that the Payload URL should match the forwarding address from [ngrok](https://blahblah.ngrok.io)
  - Select the individual events radio button and check repositories
  - Content type should be application/json
  - Save the Webhook
- Create a repository
- See that branch protection and an issue was created for the repo!

## Related Documentation
- [GitHub APIv3](https://developer.github.com/v3/)
- [Web Hooks](https://developer.github.com/webhooks/)
- [API Status](https://www.githubstatus.com/)
- [Flask Docs](https://flask.palletsprojects.com/en/1.1.x/)
- [ngrok](https://ngrok.com/docs)

## Dependencies and Attribution
- Python
- Flask
- ngrok

## Bugs and improvements
- Payloads are capped at 25 MB. If your event generates a larger payload, a webhook will not be fired. This may happen, for example, on a create event if many branches or tags are pushed at once. We suggest monitoring your payload size to ensure delivery. See [webhooks docs](https://developer.github.com/webhooks/)
- There is a 1 second Delay built in to the code which is NOT ideal. It seems the code is checking for the main branch before it is finished creating.
- This could be done with AWS Lambda and an API Gateway.

