# Miner Tool Guide

## Getting Started

This miner tool listens to the tasks come from Swan platform. It provides the following functions:

* Start arial2 download service for downloading tasks.
* Import deals once download completed.
* Synchronize deal status with Swan platform, so client will know the realtime status changes

### Prerequisites

```
sudo apt install python3-pip
pip3 install -r requirements.txt 
sudo apt install aria2
```

### Config

For aria2.conf

- **rpc-secret:**  default: my_aria2_secret. It will be used in the config.toml for rpc.

For config.toml

[main]

- **api_url:** Swan API address. For Swan production, it is "https://api.filswan.com"
- **miner_fid:** Your filecoin Miner ID
- **expected_sealing_time:** The time expected for sealing deals. Deals starting too soon will be rejected.
- **import_interval:** Importing interval between each deal.
- **scan_interval:** Time interval to scan all the ongoing deals and update status on Swan platform.
- **api_key & access_token:** Acquire from [Filswan](https://www.filswan.com) -> "My Profile"->"Developer Settings". You
  can also check the [Guide](https://nebulaai.medium.com/how-to-use-api-key-in-swan-a2ebdb005aa4)

[aria2]

- **aria2_download_dir:** Directory where offline deal files will be downloaded for importing
- **aria2_conf:** Aria2 configuration file location
- **aria2_host:** Aria2 server address
- **aria2_port:** Aria2 server port
- **aria2_secret:** Must be the same value as rpc-secre in aria2.conf


### Run Aria2 as System Service

```shell
sudo mkdir /etc/aria2
sudo chown $USER:$USER /etc/aria2/
touch /etc/aria2/aria2.session
git clone https://github.com/nebulaai/swan-miner
cp swan-miner/filswan_miner/aria2.conf /etc/aria2/
sudo cp swan-miner/filswan_miner/aria2c.service /etc/systemd/system/
# Change User and Group in the [Service] section of the aria2c.service file
sudo systemctl enable aria2c.service
sudo systemctl start aria2c.service
```

### Start swan_miner
```python3
import filswan_miner
from filswan_miner import swan_miner as miner


if __name__ == "__main__":

    miner.start("/path/to/config.toml")
```
