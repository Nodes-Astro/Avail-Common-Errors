# Avail-Common-Errors
## Common error solutions for avail node operators


![image](https://github.com/Alping0/Avail-Common-Errors/assets/105454859/e979eb42-6088-44ed-968c-8b15a1253dba)

If you see this error that caused there are something wrong with your systemd file in path section.

```
nano /etc/systemd/system/availd.service
```
Rearrenge your path according to your guide which you follow, should be like this:

```
ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain goldberg --name "Moniker"
```
or if you setup your node in home directory should be like this:

```
ExecStart= /home/"username"/avail/target/release/data-avail --base-path `pwd`/data --chain goldberg --name "Moniker"
```

![image](https://github.com/Alping0/Avail-Common-Errors/assets/105454859/cc526b28-42c5-4b06-92a1-3300074ad388)

If you see this error you should run this commands:

```
git checkout .
```
```
git checkout v1.8.0.2
```

![image](https://github.com/Alping0/Avail-Common-Errors/assets/105454859/ca00cc92-637c-412e-8730-290128aefdd0)

If you couldn't monitor your cpu usage and network traffic in grafana dashboard you should upgrade your grafana and prometheus to the latest versions:

```
sudo apt-get upgrade -y prometheus prometheus-node-exporter
sudo apt-get upgrade -y grafana
```
![image](https://github.com/Alping0/Avail-Common-Errors/assets/105454859/f91bc93c-82f5-44cc-99b7-955bd5bebdfc)

If you see connection refused error try:

```
sudo ufw allow 9944
```
If its not working your port could be used for another operation, to learn that:

```
sudo netstat -tulpn | grep avail
```
![image](https://github.com/Alping0/Avail-Common-Errors/assets/105454859/799a123f-9cfc-4a31-b246-cbb486c53154)

In the example above this node use 35549 to speak TCP in avail so you have to learn yourselves and change it with 9944, so new command should be like this: 

**Don't change anything if it works by default**

```
cd avail
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:35549
```
If you use docker you can run:

```
docker exec -it $(docker ps | grep -i availj | awk '{print $NF}') bash -c 'curl -H "Content-Type: application/json" -d '\''{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}'\'' http://localhost:9944/'
```

![image](https://github.com/Alping0/Avail-Common-Errors/assets/105454859/4bd72a8b-6678-43d9-affd-b59f7459b718)

If you saw same number while download your node looks like stuck don't worry, wait patiently sometimes it caused delay according to your vps providers network traffic. Don't exit, wait until complete.

"Replace the path with your actual data path" error
This is the error encountered by validators who dropped from active set.
You should create another folder and run again. 

```
 cd avail
systemctl stop availd.service
mkdir -p output1
cargo run --locked --release -- --chain goldberg -- validator --d ./output1 
```
and open file service and edit the path 
```
nano /etc/systemd/system/availd.service
```
```
ExecStart= /root/avail/target/release/data-avail -d ./output1 --chain goldberg --validator --name "yournodename"
```














