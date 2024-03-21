# Installing-Apache-In-Ubuntu-Server

Scenario: Level Up Bank is continuing their efforts to migrate everything over to the cloud. As a junior cloud engineer at Level Up Bank, my team was tasked with creating a new web server using Ubuntu, an open-source Linux distro widely used for web servers.

The web server will host the bank’s website, providing information about its services and allowing customers to access their accounts online.

Project Overview: This project has 3 tiers of difficulty. Foundational, Advanced and Complex. I will be challenging myself to complete all 3 tiers and will be breaking down each tier’s goals and steps.

# Tier 1: Foundational

The goals for this tier are:

— Update all packages on the server
— Install an Apache HTTP Web Server
— Enable the Apache Web Server
— Find the public IP of your server, post it in your browser to verify your webserver is working. Then post it to the Peer Review channel to verify if we can access that test webpage over the internet by members of your cohort!
— Remember to modify your AWS Security group to allow HTTP traffic.
Hint: Make sure to look up the correct username to use when SSHing into the instance.

Now that we are aware of what is needed, let’s get this project started!!!

The first thing we need to do is to log into AWS and navigate over to the EC2 console. Once there, we will need to click on Launch instances.

Give the server a name and make sure to select Ubuntu. Leave Instance type as is and create a new key pair.

![Snipe 1](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/b89fa274-f937-4beb-8ba5-35260f96be03)

![Snipe 2](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/2a16513a-c7d5-40c5-88e4-82339693528e)

In the Network settings, we will be creating a new security group allowing both SSH and HTTP access to the web server from Anywhere 0.0.0.0/0

![Snipe 3](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/5a1ecd98-56a8-456e-b7b4-cf20929351a7)

Now click on Launch instance and wait a few moments before attempting to SSH into it.

Now right click on the instance and click on Connect. The click on the SSH client tab for instructions on how to SSH into the instance.

I will be using PowerShell as my SSH client.

The first thing you need to do after launching the SSH client is to navigate to the directory where you saved the key pair to. In my case it was the download folder. (Since I am using a Windows laptop, I do not not need to run the chmod 400 command on my key pair)

Once there, copy the Example SSH command from the SSH client tab located in the Connect to instance screen.

![Snipe 4](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/f7eb6c23-e93c-4471-b230-0b6e0ef1a1b7)

Copy it into your SSH client and hit enter.

If done correctly, you should see a similar message:

![Snipe 5](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/aba57f56-2a54-44ca-b640-e114db094786)

As you can see in the screenshot, their are updates available. The first server requirement is to update all packages. In order to do this we can run the sudo apt update command to update the repos. Once that command finishes, run this command to actually install all available updates sudo apt upgrade

![Snipe 6](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/9cd557e4-bc47-4647-b37c-8ca0d8d58381)

![Snipe 7](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/be7babde-f8d2-4a54-acf2-8fb76816e9ff)

Run the sudo apt list — upgradable command to verify that all packages have been updated and installed.

![Snipe 8](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/b8b60476-35f0-4c4f-82cb-8443973df775)

The next thing we need to do is install Apache Web Server and then enable it. We can accomplish this by running the following command:
sudo apt install apache2

We can start and enable Apache by running sudo systemctl start apache2 then sudo systemctl enable apache2

if at any moment you would like to check Apache’s current status, you can run sudo systemctl status apache2

![Snipe 9](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/d2ac9a06-da70-4a84-9d7a-74ccd987f43d)

Now let’s verify that Apache is running and can be accessed through the internet. Grab the Public IP address for the instance in the EC2 console then paste it into a browser.

![Snipe 10](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/ed00ac87-51e2-4285-80f7-122a254f2820)

This concludes all of the requirements for Tier 1. Let’s start working on Tier 2.

# Tier 2: Advanced

The one goal for this tier is to create a custom HTML page that displays “Welcome to LUIT — Silver Team”

To start, the index.html that I will be using is the same one from our first project but slightly modified. I used VS Code to modify that file.

The index.html file must be placed on the EC2 instance in the following directory: /var/www/html

Typing ls / into our SSH client will display files and folders located in the current directory.

You can create the index.html file by type touch index.html then type vim index.html then make neccessary changes and save but I will be doing things a little differently..

I made a GitHub repo just for the modified index.html file then cloned the repo over to my Ubuntu web server. I will be overwriting the current index.html file with the modified one.

![Snipe 16](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/0f30e764-d131-4812-b1ab-646fb232a316)

![Snipe 17](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/079790e2-22aa-4ad0-92e4-9c7441fb94eb)

Now all we have to do is run the sudo cp /var/www/LUITProject3/index.html /var/www/html/ command to copy and overwrite the existing file with the modified one.

Now we test the website again by pasting the Public IP into the browser again….

![Snipe 12](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/2cafb631-5231-45e9-af53-ceabf10d0738)

This next step was actually from Tier 1. Since I just customized the site, I wanted to share it with my team at LUIT and it was verified to be up and functional.

We have completed Tier 2. Let’s go to the last Tier now.

# Tier 3: Complex

Scenario 2: Users have been reporting issues with the website. We need to review the logs to see if we can spot any errors.

The goals for this tier are:

— Need to use the correct command to view the latest 15 entries from the Access Logs as well as the Error Logs.
— Redirect those same Logs into each of their own files. You can use the following naming convention AccessLogs[date].txt & ErrorLogs[date].txt

Apache access logs are located in /var/log/apache2/access and Apache error logs are located in /var/log/apache2/error

Change directory into the apache2 folder. If you ls here, you will see both access.log and error.log files.

![Snipe 13](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/09f102b1-071f-47ec-a2c1-ac098e4a0a60)

Now we need to view the latest 15 entries from each of those 2 log files. let’s run the tail -n 15 access.log which would allow us to see the last 15 entries.

![Snipe 14](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/bc0bd7e2-b150-4137-94b4-8a7b89392b8b)

Now we will run the same command but for the error log (tail -n 15 error.log)

The very last requirement of tier 3 is to redirect both log files into their own files using the naming convention AccessLogs[date].txt & ErrorLogs[date].txt.

While in the /var/log/apache2 folder, I ran the following command (screenshot below) to redirect the last 15 lines of the access.log and error.log files to their own files (there is a difference in the screenshot output due to stopping the EC2).

![Snipe 15](https://github.com/Mirahkeyz/Installing-Apache-In-Ubuntu-Server/assets/134533695/5e7a6df4-c52b-4f7b-ad8b-ce33783c510f)













































































