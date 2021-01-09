#

``` bash
# install mount type
sudo apt-get install cifs-utils
# create mount folder
# sudo mkdir /mnt/file_share
# note the root of the server could not be mount
sudo mount -t cifs  //{server_name_or_ip}/{share_folder_path} /mnt/file_share -o username={username}
# then input password when ask
```
