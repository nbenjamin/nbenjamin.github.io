---
layout: post
title: Application Deployment Strategies
categories:
  - Blue/Green
  - A/B Testing
  - Canary Testing
  - OpenShift
comments: true
---

There are number of strategies deploy application in to production environment. Today we are going to test all these different strategies in OpenShift platform.(I am using mini version of OpenShift called MiniShift in my mac) 

Pre-Requisites for the following test,

	1. Install MiniShift
		 brew cask install minishift
	2. After the successful installation, start OpenShift locally
		 minishift start --vm-driver virtualbox

	Once minishift started successfully, you will see similar details in your console,
  
	Note:  The IP is dynamically generated for each OpenShift cluster. To check the IP, run the minishift ipcommand.
  
	The server is accessible via web console at:
	    https://192.168.99.100:8443/console
	
	You are logged in as:
	    User:     developer
	    Password: developer
	
	To login as administrator:
	    oc login -u system:admin


Here we are going to discuss the following options

**Blue/Green**
  
Blue/Green deployment is a technique that reduces downtime and risk by running two identical production environments called Blue and Green. At any time, only one of the environments is live, with the live environment serving all production traffic.
	
To test Blue/Green deployment login to the above URL with developer account

1. Click on the Nginx icon
<img width="1132" alt="Screen Shot 2019-09-11 at 1 32 31 PM" src="https://user-images.githubusercontent.com/870715/64720523-ab907780-d498-11e9-8924-6ea66b8e2c06.png">


2. Click Next fill all the details for project (Below screen will when you click advanced)
Git URL - https://github.com/nbenjamin/deployment-examples.git Branch - Blue

<img width="1388" alt="Screen Shot 2019-09-11 at 2 46 57 PM" src="https://user-images.githubusercontent.com/870715/64725698-1e9eeb80-d4a3-11e9-9742-7a9080fb0b18.png">

3. Click on `Continue to the project overview`
<img width="1560" alt="Screen Shot 2019-09-11 at 2 49 19 PM" src="https://user-images.githubusercontent.com/870715/64725830-5efe6980-d4a3-11e9-9179-e694a7fb8651.png">

4. Now click on the url from right side.
<img width="1445" alt="Screen Shot 2019-09-11 at 2 50 58 PM" src="https://user-images.githubusercontent.com/870715/64726000-b8669880-d4a3-11e9-9560-8af6422c4557.png">

The page will display text in blue:
<img width="680" alt="Screen Shot 2019-09-11 at 2 52 47 PM" src="https://user-images.githubusercontent.com/870715/64726177-10050400-d4a4-11e9-8d2f-33839aa576b9.png">

So Now we running version, its time to update the applicaiton with newer version

5. Repeat the step number 1
<img width="1132" alt="Screen Shot 2019-09-11 at 1 32 31 PM" src="https://user-images.githubusercontent.com/870715/64720523-ab907780-d498-11e9-8924-6ea66b8e2c06.png">

6. Click Next fill all the details for project (Below screen will when you click advanced)
Git URL - https://github.com/nbenjamin/deployment-examples.git Branch - Green
<img width="1459" alt="Screen Shot 2019-09-11 at 3 06 12 PM" src="https://user-images.githubusercontent.com/870715/64726967-bef60f80-d4a5-11e9-807f-7ee36f53bbfc.png">

7. Contiue the step till we get the deployed URL and you should see something like this
[New-URL](http://bluegreendeploy-new-myproject.192.168.99.100.nip.io)


<img width="670" alt="Screen Shot 2019-09-11 at 3 10 03 PM" src="https://user-images.githubusercontent.com/870715/64727175-45125600-d4a6-11e9-922f-5dda6aa997f3.png">

Once testing is over, its time to switch over to the new version

8. To enable the routing, click `Application menu-> rounter ` and select `bluegreendeploy` application
<img width="1532" alt="Screen Shot 2019-09-11 at 3 14 26 PM" src="https://user-images.githubusercontent.com/870715/64727453-dc77a900-d4a6-11e9-9213-0e2b1505906f.png">

9. Now click on Edit from Action 
<img width="1416" alt="Screen Shot 2019-09-11 at 3 19 03 PM" src="https://user-images.githubusercontent.com/870715/64727767-98d16f00-d4a7-11e9-8f61-89a69b1eae1e.png">

10. Select the right service and Save.

<img width="527" alt="Screen Shot 2019-09-11 at 3 20 53 PM" src="https://user-images.githubusercontent.com/870715/64727861-d0d8b200-d4a7-11e9-9384-1472d54a16a7.png">

11. Now Old URL will have the latest (Green) changes
[URL](http://bluegreendeploy-myproject.192.168.99.100.nip.io/)
<img width="979" alt="Screen Shot 2019-09-11 at 3 22 59 PM" src="https://user-images.githubusercontent.com/870715/64727972-0ed5d600-d4a8-11e9-9cdd-e8a9b0816acb.png">




    
**Canary**
