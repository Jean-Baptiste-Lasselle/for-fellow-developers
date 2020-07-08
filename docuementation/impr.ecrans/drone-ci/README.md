# About Drone CI 

 * Drone detects events either from github or gitlab , using on github, and gitlab , the concept of an "application" : 
   * in short when you create an _application_ for your github or gitlab user, you get a client ID and a client Secret which Drone can use to operate in collabroation with git repo events
   * see on github when you create the "drone ci" app, you have to give the app, the URL of your _application_ (Dorne cI is the _application_ here)  that's because github is gonna send stuff to the registered _applicatuion_, typically webhooks. Now we know how drone knows that we have pushed a commit to a branch on our git repo.
   
![gtihub app client id client secret and APP WEBHOOK ENDPOINT URL](https://github.com/Jean-Baptiste-Lasselle/for-fellow-developers/raw/master/docuementation/impr.ecrans/drone-ci/DRONE_CI_GITHUB_APPLICATION_REGISTRATION_FOR_CLIENT_ID_CLIENT_SECRET_Firefox_Screenshot_2020-07-08T17-49-13.072Z.png)
