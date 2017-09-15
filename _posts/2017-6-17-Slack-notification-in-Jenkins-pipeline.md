---
layout: post
title: Slack notification in Jenkins pipeline
---

I'm starting to approach to CI/CD world and I found [Jenkins](https://jenkins.io/) is a good tool.

As first step I've created a lot of jobs to perform various steps of CI/CD pipeline, but now I discovered Jenkins pipeline features...and that was great!
I could refactor all jobs in order to have under control all steps of deployment.

Bw the way, it is not feasible to have a browser tab always open to Jenkins pipeline UI, so best practice is to include some notification steps into your pipeline script.

I choose to use Slack to notify when pipelines completes successfully or fails.
To do that, it is necessary to have installed Slack plugin into you Jenkins server; this plugin allows to use `slackSend`
step into your stages.

For example,

```groovy
dir('repository') {
  checkout changelog: false, poll: false, scm:[$class: 'GitSCM',
    branches: [[name: '*/master']],
    doGenerateSubmoduleConfigurations: false, extensions: [],
    submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'blahblahblah',
    url: 'git@github.com:my-org/repository.git']]]

    slackSend channel: '#your-channel', color: 'good',
      failOnError: true, message: '<!here> ' + env.JOB_NAME + ' build #' +
        env.BUILD_NUMBER + ' (<' + env.BUILD_URL + '|SCM clone>)
        - Job completed', teamDomain: 'your-team', token: 'yourtoken'            
}
```

clones a git repository and send a message to Slack. To customize your Slack messages, you could read [this article](https://api.slack.com/docs/message-formatting).
