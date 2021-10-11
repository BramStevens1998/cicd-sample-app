# Lab Report: Lab 2 Cicd

## Lab
### Jenkins Passwd
649ad398de1849468e8a65e196e923f2

### Deel 2.3

- passwd ophalen
```console
vagrant@dockerlab:~$ docker exec -it jenkins_server /bin/cat /var/jenkins_home/secrets/initialAdminPassword
649ad398de1849468e8a65e196e923f2
```
### 2.4 
- admin aanmaken (passwd = oud bd)

![Admin, 11 october 2021](img/JenkinsAdmin.PNG?raw=true)

- instance config
```console
http://192.168.56.20:8080/
```



## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.
