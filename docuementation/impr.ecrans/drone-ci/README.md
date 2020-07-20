# About Drone CI 


## On premises 

 * Drone detects events either from github or gitlab , using on github, and gitlab , the concept of an "application" : 
   * in short when you create an _application_ for your github or gitlab user, you get a client ID and a client Secret which Drone can use to operate in collabroation with git repo events
   * see on github when you create the "drone ci" app, you have to give the app, the URL of your _application_ (Dorne cI is the _application_ here)  that's because github is gonna send stuff to the registered _applicatuion_, typically webhooks. Now we know how drone knows that we have pushed a commit to a branch on our git repo.
   
![gtihub app client id client secret and APP WEBHOOK ENDPOINT URL](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_GITHUB_APPLICATION_REGISTRATION_FOR_CLIENT_ID_CLIENT_SECRET_Firefox_Screenshot_2020-07-08T17-49-13.072Z.png)


## The free plan on https://cloud.drone.io

* Create the github application for the `pegasus-io` github organization  : with cloud.drone.io , the autoregister at login, creates the github application that will be used. 
* All you need to do, is  to grant the application (as you create it), the required github permissions on the github organization that contains the git repo : 

![permissions on pegasus-io github org](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_IN_MY_REPOS_STEP_3_PERMISSIONS_ON_ORGANIZATION_2020-07-20T16-38-47.685Z.png)

* After that, in your drone.io dashboard, in the github repositories list, I activate the drone pipeline, and get to its configuration page : 

![pipeline config](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_IN_MY_REPOS_STEP_5_PERMISSIONS_ON_ORGANIZATION_ACTIVATE_AND_CONFIGURE_PPIPELINE_2020-07-20T16-41-37.166Z.png)

* first settings are to set the drone-ci yaml file name, and a few other settings related to github webhooks and how they trigger the pipeline : 

![triggers and yaml filename](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_IN_MY_REPOS_STEP_6_2020-07-20T16-43-47.702Z.png)
* Among those first settings, one is about an interesting drone ci feature : 
  * Some files, like a configuration file, can be signed, and its signature checked to be signed by a verified `GPG` signer 
  * if the signature is verified, then the pipeline execution can happen. If `GPG` signature verification fails, pipeline execution is cancelled. 
  * signature's  `signingKey` has to be configured in drone as well, and [this is done using the `.drone.yml` file](https://docs.drone.io/signature/) : 


```Yaml
---
kind: pipeline
type: docker
name: default

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test

---
kind: signature
hmac: F10E2821BBBEA527EA02200352313BC059445190

...
```

  * the verification can be enabled/disabled using the "protected" labelled checkbox below : 

![protected](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_YAML_FILE_SIGNATURE_PROTECTION_2020-07-20%2019-14-52.png)

  * This can be integrated with git commit signature :  
    * the git `signingKey` signs commits, not files, but we can add a signature's public key in the `.drone.yml` file , that is expected to be equal to the git commit signer. 
    * So that the git `signingKey`  has to be equal to the public key  configured in the `.drone.yml` file, and the public key  configured in the `.drone.yml` file has to verify the commit of the GIT version being run in the pipeline/


* Then you will configure secrets and schedule recurring pipeline execution ("cron job") : 

![secrets and cron jobs](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_IN_MY_REPOS_STEP_7_secrets_and_cron_jobs_2020-07-20T16-45-41.699Z.png)

* then you can get the drone.io badge for the project: 

![drone.io badge](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_IN_MY_REPOS_STEP_8_badges_2020-07-20T16-46-49.658Z.png)

```
[![Build Status](https://cloud.drone.io/api/badges/pegasus-io/this-is-gravitee/status.svg)](https://cloud.drone.io/pegasus-io/this-is-gravitee)
```

* so my first `.drone.yml` is : 

```Yaml
kind: pipeline
# you could have [type: kubernetes]
type: docker
name: default

steps:
- name: testpipeline1
  image: alpine
  commands:
  - echo 'This'
  - echo 'is'
  - echo 'Gravitee!'
- name: testpipeline2
  image: alpine
  commands:
  - echo "We are in [$(pwd)]"
  - ls -allh
  - echo 'ok!'
```
* I create a new feature branch with the gitflow, named `drone_ci_setup`
* I create, add, commit and push the additional `.drone.yml` file, and the badge in the `README.md` : for each commit on my branch feature, the drone pipeline will execute the pipeline on the feature branch, on the commit that was just pushed  : 

![trigger pipelines on commit on feature branch](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_TRIGGER_ON_FEATURE_BRANCH__COMMITS_2020-07-20T18-19-13.284Z.png)

* I create a pull request from the `feature/drone_ci_setup` branch, to the `develop` branch : 
  * this will trigger the Drone Pipeline, and what is interesting here, is that when the triggering event is a pull/merge request, drone will "simulate", the merge into the pipeline, without pushing to original repo : 

![merge simulation on pull/merge requests](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_MERGE_SIMULATION_ON_PULL_REQUESTS_2020-07-20T18-03-51.721Z.png)

  * and there you go for the pipeline execution mentioned in the github pull request : 

![pull requests github and drone pipeline execution](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/GITHUB_PULL_REQUEST_AND_DRONE_PIPELINES_2020-07-20T17-59-17.423Z.png)

  * all in all, I had three pipeline execution , 2 for the 2 commits on my feature branch, and one for the pull request (which simulates merge to develop) : 

![pipeline execution for drone ci setup](
https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_SETUP_PIPELINE_EXECUTION_2020-07-20T18-17-48.949Z.png)

