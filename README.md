# jenkins_triggers
Repository to test automatic pipeline triggers

1. To Avoid the Error "Host key verification failed", First go to Jenkins -> Verwaltung > Configure global Security -> scroll down until "Git Host Key Verification Configuration" 
    and select the first option ("Accept first connection)  
2. Go to Jenkins, create a new pipeline, select "pipeline script from SCM" -> Source Code Management, in this case Git
[PROBLEM SOlVED]: if on jenkins pipeline configuration you get the error "Permission denied (public key)" while testing the connection to
    github repository, it means you need to generate the key-pair for jenkins user and upload the public key on github:
    - Log in to the server where jenkins server is running
    - Log in as jenkins user: `sudo su - jenkins`
    - Check if the key pair exists, otherwise generate it:
       `ssh-keygen`
    - Upload the public key into the list of known keys on github
    - Try again
   
Most popular triggers:
    - Github Webhooks:
      - In order to let the pipeline start whenever a new push into the repository is done, configure the webhooks:
        1. go to `https://github.com/centerba70/jenkins_triggers/settings/hooks/new`
        2. specify as Payload URL the url where jenkins server is reachable in this form:
           `http://localhost:8084/github-webhook/`
        [PROBLEM SOLVED] Github will complain, that the provided url with localhost will not be reachable over the internet;
            Use grok to solve this issue and redirect the provided external url to localhost:
            - Install grok: `https://ngrok.com/download/linux` and add also the authtoken
            - Start the endpoint: `ngrok http 8080` --> look in the provided information after the execution of this command,
              and use the given url for the github webhooks; all the calls will be redirected to localhost:8080
        3. Go back to jenkins, select the build job -> Configure -> Triggers -> enable "GitHub hook trigger for GITScm polling"
            