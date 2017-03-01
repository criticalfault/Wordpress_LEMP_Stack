#Deployment Practices
<hr>
##Installation Process
Find a Link here for installation process. [Link](https://github.com/criticalfault/Wordpress_LEMP_Stack/blob/master/setup.md)

<br>

##Table of Contents
<hr>
1. [Adding a Favicon](#Adding-Favicon's-to-Wordress)<br>

<br>

##Adding Favicon's to Wordress

1) Download a file you want to make your Favicon. File format should be ICO, however wordpress will accept any image if it is the correct pixel size. Navigate on your local machine to the location of that file on your local machine's terminal. The file should be at maximum, 151x151 pixel's in size. 

2) Now we are going to be transferring the file using SCP from our local machine to the remote server. We do this with first specifying the file, then the user@hostname, followed by a : and the EXACT file path location the file will end up, ending with the file name itself. It must be placed inside the upload's directory we create inside the 'root document' folder that wordpress is being served from.

```shell
scp favicon.ico root@138.197.116.86:/var/www/html/favicon.ico
```

3) Once the file has been transferred to the document base of our project Nginx will serve this by default. We can easily change this by simply replacing this file or remove the favicon by simply deleting the file. The favicon is saved in the browsers memory however, so it will persist for some users even after being deleted. If it is replaced it, it will be replaced on their browser immediately.

