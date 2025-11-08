Next Challenge: Build this same pipeline using Github actions and Gitlab CI

# I pretty much followed what Prashant Gohel mentioned in the redame.md file. Below are the obstacles I faced while following those steps and how I resolved them:

1) Updating and upgrading the linux packages, installing git, docker, docker.io and docker compose, Installing Jenkins and post-installation setup wizard by referring- https://www.jenkins.io/doc/book/installing/linux/ on EC2 instance.
Here you get to see the Jenkins password by using command: $ sudo cat /var/lib/jenkins/secrets/initialAdminPassword

2) Remember to access jenkins from EC2 instances' IP. You can get public IP using: $curl ifconfig.me.

3)Create a git repo for the project on Github. Cloned Prashant's repo into my local machine. Added and commited it. 
Before pushing it remove the remote configured previously- $ git remote remove origin and configure your repo as remote by using- $ git remote add origin < URL of your git repo>

4) As Dockerfile, docker-compose.yaml and JenkinsFile are already present in repo we do not need to re-write it again. I went through it. In Dockerfile the application is packagd with its dependencies and is ready to be built.
   In docker-compose.yml the app is built into an image. And a ready made image of my-SQL is referenced from dockerhub for db. Once we do docker compose up these two containers will be running.

5) In Jenkins my pipeline failed twice: For resolving first error I changed 'Branch Specifier' to */main instead of */master. And for the second error I added jenkins to docker group: 'sudo usermod -aG docker jenkins' and restarted Jenkins: sudo systemctl restart jenkins. Now you can see jenkins being added using command - $ groups jenkins.

6) I had to change EC2 instance type from t3.micro to t3.small as the jenkins pipeline wa rnning on t3.micro. Stop the instance and from Actions->Change Instance type. Still Jenkins runs very slow but be patient with it and it builts the pipeline. 

<img width="1792" height="1068" alt="Screenshot 2025-11-08 at 15 26 48" src="https://github.com/user-attachments/assets/1eb998a4-9db1-42a1-8fd4-0059d9a3891c" />


<img width="1792" height="840" alt="Screenshot 2025-11-08 at 16 04 17" src="https://github.com/user-attachments/assets/dca290e1-4f00-4f4a-80e5-0494d258022f" />

ubuntu@ip-172-31-24-50:~/DevOps-Project-Two-Tier-Flask-App$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS                      PORTS                                                    NAMES
749e78532a53   jenkins-pipeline-flask-app   "python app.py"          58 minutes ago   Up 58 minutes (unhealthy)   0.0.0.0:5000->5000/tcp, [::]:5000->5000/tcp              two-tier-app
c4ddaf314239   mysql                        "docker-entrypoint.s…"   58 minutes ago   Up 58 minutes (healthy)     0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp   mysql
