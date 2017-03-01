#Deployment Practices
<hr>

Table of Contents
<hr>
1. [Adding a Favicon](#Adding Favicon's to Wordress)<br>
2. [Second Task](#Second-Task)<br>
3. [Third Task](#Third-Task)<br>
4. [Fourth Task](#Fourth-Task)














##Adding Favicon's to Wordress

1) Download a file you want to make your Favicon. File format should be ICO, however wordpress will accept any image if it is the correct pixel size. Navigate on your local machine to the location of that file on your local machine's terminal. The file should be at maxium, 151x151 pixel's in size. 


2) Now we are going to be transfering the file using SCP from our local machine to the remote server. We do this with first specifying the file, then the user@hostname, followed by a : and the EXACT file path location the file will end up, ending with the file name itself. It must be placed inside the upload's directory we create inside the 'root document' folder that wordpress is being served from.

```shell
scp favicon.png wordpress@138.197.116.86:/var/www/html/wp-content/uploads/favicon.png
```

3) Once the file has been transfered to the uploads section, wordpress' file system will be able to recognize the file as a legal target to be choosen to be assigned the favicon. Now we need to log into Wordpress' dashboard by navigating to this address

```shell
http://138.197.116.86/wp-login.php
```
Enter the "Administrator" account's password. This will bring you back to the front page of wordpress with the extra "admin bar" across the top. Click on the dropdown bar and select "Dashboard"

![Imgur](http://i.imgur.com/X9kqnQb.png)

5) Click on Appearance and then Customize...

![Imgur](http://i.imgur.com/fRSEufW.png)

6) Click on site identity...

![Imgur](http://i.imgur.com/chIddCD.png)

7) Click select image at the bottom of the screen

![Imgur](http://i.imgur.com/uCV0uyp.png)

8) Select the file from the list of files. These files live in the 'uploads' directory we created.

![Imgur](http://i.imgur.com/WX0aq85.png)

9) Finally click select and then when your prompted with the new page. Click Save & Publish. This has now changed your favicon!

![Imgur](http://i.imgur.com/QAf1BK4.png)


##Second Task

