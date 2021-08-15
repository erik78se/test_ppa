# Ubuntu/debial repo for Kilt packages (develop)
This repo contains packages for
* kilt-parachain
* mashnet-node

Below is information how to install the kilt packages and run the services. (Also some aux information about how this repo works as a deb repo). 

Note. This repo will go away soon as the packages will move to a more permanent location. Use this primarily for test.

## Supported OS:es
Currently, the built packages are supported on:

* Ubuntu focal

## Add this repo to your hosts:

    curl -s --compressed "https://erik78se.github.io/test_ppa/KEY.gpg" | sudo apt-key add -
    sudo curl -s --compressed -o /etc/apt/sources.list.d/my_list_file.list "https://erik78se.github.io/test_ppa/my_list_file.list"
    sudo apt update

## Installing
You can install both or one of them.

    apt install kilt-parachain mashnet-node

### Configuring
Daemons startup configurations for the servces are located in:

   /etc/default/kilt-parachain
   
   /etc/default/mashnet-node

The services will run as a system user "kilt" with no login capability.

The files and user kilt home is located in: 

    /var/lib/kilt/

### Starting/stopping
Both daemons are started with systemd

    systemctl start kilt-parachain
    
    systemctl start mashnet-node    


Check logs with:

    journalctl -xe -f -u kilt-parachain.service

    journalctl -xe -f -u mashnet-node.service


## Add packages to this repo

    dpkg-scanpackages --multiversion . > Packages
    gzip -k -f Packages

    # Release, Release.gpg & InRelease
    apt-ftparchive release . > Release
    gpg --default-key "${EMAIL}" -abs -o - Release > Release.gpg
    gpg --default-key "${EMAIL}" --clearsign -o - Release > InRelease

    # Commit & push
    git add -A
    git commit -m update
    git push

# Packaging into github Pages
https://assafmo.github.io/2019/05/02/ppa-repo-hosted-on-github.html
