# Deploy Shiny Instructions

## License

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

### You are free to:

- Share — copy and redistribute the material in any medium or format
- Adapt — remix, transform, and build upon the material 

## Under the following terms:


- Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
- NonCommercial — You may not use the material for commercial purposes.
- ShareAlike — If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original. 

## Instructions

### Step 1: Download Shiny Server

**Check that this is the latest link**

```
wget https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.7.907-amd64.deb
```

### Step 2: Install Shiny Server

```
dpkg -i shiny-server-1.5.7.907-amd64.deb
```

### Step 3: Give Shiny Server Permissions

```
chmod 777 /srv/shiny-server
```

### Step 4: Make Directories for the Server

```
mkdir /srv/shiny-server/{APP_NAME}/
mkdir /srv/shiny-server/{APP_NAME}/data/
mkdir /srv/shiny-server/{APP_NAME}/www/
```

### Step 5: Copy App contents to the Server

```
cp *.R /srv/shiny-server/{APP_NAME}/
cp ./data/*.csv /srv/shiny-server/{APP_NAME}/data/
cp ./www/*.png /srv/shiny-server/{APP_NAME}/www/
```

### Step 6: Create Configuration File

Filename: **shiny-server.conf**

You have to specify the port for your application. 

```
# Instruct Shiny Server to run applications as the user "shiny"
run_as shiny;

#Preserve logs
preserve_logs true;

# Define a server that listens on port XXXX 
server {
  listen XXXX;

  # Define a location at the base URL
  location / {

    # Host app stored in this directory
    app_dir /srv/shiny-server/{APP_NAME};

    # Log all Shiny output to files in this directory
    log_dir /var/log/shiny-server;

    # When a user visits the base URL rather than a particular application,
    # an index of the applications available in this directory will be shown.
    directory_index off;
  }
}
```

### Step 7: Copy the Configuration File to the server's path

```
cp shiny-server.conf /etc/shiny-server/
```

### Step 8: Start the Server in Detached mode

```
/usr/bin/shiny-server &
```
