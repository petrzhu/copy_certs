# copy_certs
Python script for "certbot renew" post-hook copy of certifications with the purpose of automating of the process of updating SSL certificates for jwilder/nginx-proxy container.

## Purpose
This Python script is for copying the renewed certificate files from the letsencrypt live directory to the nginx-proxy container's mounted volume with the modified names for the target container.

## Example
Example of a crontab line:
```
0 0 * * 0 certbot-auto renew --pre-hook "docker container stop nginx_proxy" --post-hook "copy-certs --cff SOURCE_FOLDER --ncf DEST_FOLDER --cvh VIRTUAL_HOST" --post-hook "docker run --rm -d --name nginx_proxy -p 80:80 -p 443:443-v DEST_FOLDER:/etc/nginx/certs:rw jwilder/nginx-proxy"
```
Where SOURCE_FOLDER is the directory of the freshly obtained certificate files,

DEST_FOLDER is the directory for mounting for the nginx container

and VIRTUAL_HOST is the environment variable VIRTUAL_HOST of the target container.

## P.S.
NOTE: some of the arguments of "docker run" command are skipped for making the example less complicated.
