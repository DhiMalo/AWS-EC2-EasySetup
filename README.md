# AWS-EC2-EasySetup
Launch a server on Amazon Web Services EC2 (Elastic Compute Cloud)  

<i> This repo is under construction. Please visit again! </i>

<strong> What is AWS EC2?</strong>
AWS EC2 is a service Amazon provides allowing developers to create, launch, and terminate server-instances as needed and pay only for active server usage time. 

First we'll launch the instance then we'll build the server.

1. First [log in to Amazon AWS](http://aws.amazon.com/). Login is nested in the "My Account" Menu.
(Image)
1. Check and set your location in upper right corner to the one nearest to you.
1. Open the EC2 Dashboard
1. Select Running Instances
1. Select Launch Instance
1. Choose an OS - I chose Ubuntu 14.04 which is free-tier eligible
1. Keep all defaults until Step 6 (Go to next, next, next until then) 
1. Step 6: Configure New Security Group - this is the first real setup you'll be doing by hand.
(Image)
1. Name and Describe the Security Group - Name it anything a firewall should be called :)
1. Review and modify your rules for access and ports:
- http (Keep this, it will appear by default)
- https (Keep this, it will appear by default)
- 8080 - Add this port sometimes used by websites.
- 9000 - Add this is for test and setup purposes.
- SSH - (Keep this, it will appear by default) - you'll use this to talk to your server like any other SSH.
1. Set the source access for EACH PORT to “Anywhere”
1. Click Review and Launch.
1. Create a new key pair
 - Name it
 - Save it in a safe place on your computer
 - Make note of the directory to which you're saving this private key!
1. Once saved, launch the instance.
 - It will take awhile to build.  
 - To look at progress, you can check out the EC2 Dashboard
 
<strong> Use the command line to access the new cloud server and program its output. For programming the server, I used Node.js</strong>
We'll need to install Node, install any packages we'd like to run, and then write the code to build our server.

1. In terminal, CD into the directory holding your new private key
-<code> cd [directory] </code>
1. Initiate a secure connection with your ubuntu instance:
-<code> ssh -i ubuntu@public-DNS-for-the-EC2-instance-you-just-launched</code>
- The public DNS can be found at the bottom right corner of your EC2 Dashboard.
1. If this command is denied, you need to modify permissions for the pem (key) file. (Aside at the end of this document.)
1. Now reattempt to ssh -i to the ubuntu instance.
1. Now that we’re logged into the instance, make sure its packages are updated.
…[fill]
1. Create a directory for this new file
1. CD into this newly unpacked directory
1. Run binary for this file, then make it and install it. - <code>./configure && make && sudo make install node </code>
1. Node installs itself on the ubuntu instance. Now it's time to write your server file.
1. Here is the one I am using:
[add]

1. Continuously run node makeServer.js, and put it in the background - <code>nohup node makeServer.js & </code>
1. At any time you can kill this background task with <code> kill [process#] </code>
1. Additionally, note that you can see all background processes with the script <code> ps -ef will let me </code>
1. OK visit public-DNS-for-the-EC2-instance-you-just-launched - page should be live but why don't you see it?
1. Add onto the url a colon and the port number you provided in the server file (eg. :9000) 

<strong> We want to run it at port 80, the default http port.  But we don’t want to have to sudo and use password as usual for this. </strong>

1. sudo npm install express
1. Create a new directory and a new js file inside to run an express server.
1. Kill old process <code> kill [process#]</code>
1. Write an express server. Here is the one I am using. [add]
1. Instead of changing the port to 80 and making our administrative privledges vulnerable we'll use IP Tables.
This will reroute incoming port 80 http traffic to port 9000. 
- <code> sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 9000 </code>
Now run the expressServer.js file again and even though it runs on port 9000, it also redirects to 80.

1. Run nohup again to make this process continuous!
<code>nohup node [filename].js &</code>

1. Congratulate yourself! You have a running instance pushing your server code out all the time!

---
####Aside: Modifying permissions for the pem file. 
1. Review current directory alongside permissions information - <code> ls -l </code>
1. Disable the Group membership and add access for owner to read/write only
eg. :   Owner Group Everyone
        [RW-] [---] [---]
<code> chmod 600 [Filename] </code>
