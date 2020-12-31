# Skill17-Environment

## How to create the Docker image
    docker build -t esimage .

## How to use the built image
    # change your directory to Github repository before running Docker command to make sure volume mounts are created right
    cd Skill17-Environment
    docker run -d --name esimage -p 80:80 -p 8000:8000 -p 3306:3306 -p 22:22 -p 4200:4200 -v "$(pwd)/mysql:/var/lib/mysql" -v "$(pwd)/sources:/var/www/sources" esimage:latest

(note: this command should work on Unix-based systems (Linux, OS X, WSL etc.) and Windows Powershell. If it is not working, changing `$(pwd)` to your current directory might help.)

## Run a shell within running container
    # root shell via Docker
    docker exec -it esimage bash
    
    # or SSH (competitor has superuser permissions):
    ssh competitor@127.0.0.1
    
## Make symbolic link from persistent sources directory to html webroot
Note: if you just want to use single controller (like Laravel) to handle everything or know how to Apache, you can just change your `vhost.conf`. If you change it in host repository, new image (and container) can be built with persistent change (but other changes inside container are lost).

    # Examples:
    # When created Laravel app named "backend"
    ln -s /var/www/sources/backend/public /var/www/html/api
    
    # When wanting to symlink the whole directory
    # note: you must rename (or remove) the html directory first!
    mv html html_backup
    ln -s /var/www/sources /var/www/html
    
**Warning!** Configuration changes inside container are not persisted to image. If you delete and rebuild the container, your sources and MySQL data will stay, but any symlinks and other configurations are deleted. Document your configurations at will. Modifying `entrypoint.sh` script works, too.
