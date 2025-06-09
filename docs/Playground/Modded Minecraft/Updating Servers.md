!!! warning "Backup your server data"

	Remember to backup your server before updating to prevent loss of data. This would depend on the modpack that you're using as each has their own directory structure. Usually these files would be the `world save`, `modified configs`, and the `run script`

## Which files do you need to copy?
- World save
	- Rlcraft Dregora - ./DregoraRL
	- ATM10 - ./world
- local
- server.properties
- your run script - make sure to also update the mod loader if updated
- eula.txt - so you dont have to rerun again
- configs that you've modified
## Download the latest server files
- Extract the server files to a new directory
- Copy over the files you've backed up earlier `world save`, `local`, `server.properties`, `run script`, `eula.txt`.


!!! notes

	Modify your run script if the modpack's mod loader got updated

	Don't forget to transfer your local mod data, such as JEI and Journey map, if creating a new mod profile for your host device.

	Not every modpack has a `local` folder

## Update script

Shell script that I used to update my ATM10 server. The ATM zip name were inconsistent with some updates having a dash on the version number and some with none so I had to check and rename the file before running the script.

Before running you need to first declare the server directory and have the zip file there as well. The script will accept two parameters, the current version number and the latest.

```
#!/bin/bash
#Bash script to update ATM10 Server files

#Validate input to have 2 values
if [ "$#" -ne 2 ]; then
    echo "Usage $0 <parameter_1> <parameter_2>"
    exit 1
fi

ATM_SUBFOLDER="path-to-server-folder/ATM10"

LS_DIR=("world" "local" "journeymap")
LS_FILE=("eula.txt" "server.properties")

#Check if in the ATM subfolder
if [ "$(pwd)" != "$ATM_SUBFOLDER" ]; then
    echo "Move to ATM Subfolder to run the script"
    exit 1
fi

# Assigning parameters
PARAM_1="$1"
PARAM_2="$2"

#Concatenate PARAMs to PATH
SRC_DIR="./Server-Files-${PARAM_1}/"
DST_DIR="./Server-Files-${PARAM_2}/"
BU_DIR="../backups/ATM10-${PARAM_1}/"

#Server ZIP file
SRV_ZIP="./Server-Files-${PARAM_2}.zip"

#Check if SRC_DIR exists
if [ ! -d "$SRC_DIR" ]; then
    echo "$SRC_DIR does not exist"
    exit 1
fi

#Check if DST_DIR exists
if [ ! -d "$DST_DIR" ]; then
    echo "Unzip the server files first"
    
    #File exists and is unzipped
    if [ ! -f "$SRV_ZIP" ]; then
        echo "No ZIP found, get the latest file"
        exit 1
    else
        sudo unzip "$SRV_ZIP"
    fi
fi

# Ensure backup directory exists
if [ ! -d "$BU_DIR" ]; then
    mkdir -p "$BU_DIR"
fi

#Copy directories
for dir in "${LS_DIR[@]}"; do
    if [ -d "${SRC_DIR}${dir}" ]; then
        #Make backup of dirs
        sudo cp -r "${SRC_DIR}${dir}" "${BU_DIR}"

        #Copy to new dir
        sudo cp -r "${SRC_DIR}${dir}" "${DST_DIR}"
    else
        echo "Directory ${SRC_DIR}${dir} not found, skipped."
    fi
done

#Copy files
for file in "${LS_FILE[@]}"; do
    if [ -f "${SRC_DIR}${file}" ]; then
        #Make back of files
        sudo cp "${SRC_DIR}${file}" "${BU_DIR}"

        #Copy files to new dir
        sudo cp "${SRC_DIR}${file}" "${DST_DIR}"
    else
        echo "File ${SRC_DIR}${file} not found, skipped."
    fi
done

#Make start script an executable
sudo chmod +x "${DST_DIR}startserver.sh"

echo "Transfer complete"
```