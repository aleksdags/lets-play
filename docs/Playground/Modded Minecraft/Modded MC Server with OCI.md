# Modded Minecraft Server with OCI

!!! warning

 	Modpack used here is RLCraft Dregora, setup may be different to other packs

## Get an OCI Instance
- Get an instance, use the official [oracle guide](https://blogs.oracle.com/developers/post/how-to-set-up-and-run-a-really-powerful-free-minecraft-server-in-the-cloud#create-a-virtual-machine-instance) 
	- *Always free VMs are hard to get you can use this [script](https://github.com/gardinbe/oracle-compute-instance-creation-script/tree/master) to automate the clicking.*
		- Low chance that you'll be able to get an instance with this method.
	- *If your account is new, use the trial credits and create a paid VM while you try to get an ARM VM*
		- What happens when you finally get the always free VM or when the trial is about to end?
			- You can detach the current volume before deleting the paid VM to keep your server data. 
			- **Note: Use another Linux ARM instance as you cannot reattach the boot volume of your x86 instance**
	- Easiest way to get an instance is to upgrade your account to pay-as-you-go and stick to the limits of the always free tier to avoid getting charged.
## Setting up the VM
- Get Java that supports your modpack
	- ```sudo apt install openjdk-#-jdk```
	- replace # with required Java version
- Install Forge, must be for version of desired modpack
	- **Note: NOT all modpacks uses forge, check the zip file as they usually contain notes on their mod loader**
- Open the forge .jar to install server
	- ```java -jar <forge-installer.jar> --installServer```
- Move the generated files to desired modpack
- Download modpack server zip then extract after
- Create a script that will hold the server arguments
	- Use this [script generator](https://docs.papermc.io/misc/tools/start-script-gen) for your start script
	- `nano <script.sh>`
- Ensure that the .sh script is an executable
	- ```chmod +x <script.sh>```
- Configure your firewall
	- Refer to the oracle official guide to manage OCI firewall
	- Install firewalld
	- ```sudo apt install firewalld```
	-	```
		sudo firewall-cmd --permanent --zone=public --add-port=25565/tcp
		sudo firewall-cmd --permanent --zone=public --add-port=25565/udp
		sudo firewall-cmd --reload
		```
- Start the server
	- I used **tmux** for running the server to separate the instance and allow me to use the VM for other things.
		- `tmux new -s <session_name>`
	- This makes it so that when you exit your SSH session, the server will continue to run 
		- `ctrl+b d` to exit tmux instance
		- `tmux a -t <session_name>` to rejoin tmux instance
	- Run the script ```./<script.sh>```
	- Edit the eula then restart your script
		- `nano eula.txt`
		- `eula=true`

## Backing up data using WSL

Copy the ssh key to your wsl for easier access, host volumes are located in /mnt.
```
cp <path_to_ssh_key> <desired_dst>
```

Make sure to change file permission and ownership
```
chmod 600 <ssh_key>
chown $(whoami):$(whoami) <ssh_key>
```

## Using tar to create the archive
```
tar -czf name-of-archive.tar.gz /path/to/directory-or-file
```
-c: Create an archive.

-z: Compress the archive with gzip.

-f: Allows you to specify the filename of the archive.

## Transferring data from OCI to WSL and vice versa with Rsync
From OCI to WSL
```
rsync -avz --progress --stats -e "ssh -i <ssh_key>" user@<ipadd>:<path_to_file> <dst_directory>
```
-a: Archive

-v: Verbose mode

-z: Compress files during transfer

From WSL to OCI
```
rsync -avz --progress --stats -e <transfer_file> "ssh -i <ssh_key>" user@<ipadd>:<path_to_file> 
```