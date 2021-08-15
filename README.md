# Packaging
https://assafmo.github.io/2019/05/02/ppa-repo-hosted-on-github.html

## Add this repo:

    curl -s --compressed "https://erik78se.github.io/test_ppa/KEY.gpg" | sudo apt-key add -
    sudo curl -s --compressed -o /etc/apt/sources.list.d/my_list_file.list "https://erik78se.github.io/test_ppa/my_list_file.list"
    sudo apt update

## Add package here

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


