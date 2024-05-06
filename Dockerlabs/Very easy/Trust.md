# Solving "Trust" machine from [Dockerlabs](https://dockerlabs.es/#/)

*Level: Very easy*

## Downloading the machine

Download the "Trust" file from [Dockerlabs](https://dockerlabs.es/#/) and extract it to your desktop.

![image](https://github.com/searcilas/virtual_machines/assets/105607232/3444bc00-2b27-40eb-8c44-9fb13805fa04)

## Connecting to the machine

The first thing to do is to connect to the machine via terminal using the command
```shell
sudo bash auto_deploy.sh trust.tar
```


![image](https://github.com/searcilas/virtual_machines/assets/105607232/f1e13087-c4d3-4a64-8c44-78ee68350b87)

## Deleting the machine

If you want to delete it, press Ctrl + C and it will be deleted.

*Poner imagen de cuando se este eliminando*

## Checking the machine status

Open a new terminal. Check if the machine is alive
```shell
ping -c 3 172.17.0.2
```

![image](https://github.com/searcilas/virtual_machines/assets/105607232/2c2afb2f-e651-426b-83fe-c04268fbc727)

If the ping is successful, it means the machine is alive, so we can start to play with it :D

## Scanning opened ports

To do this let's use Nmap. If your Linux distribution has not nmap installed don't worry. Instal it using the command `sudo apt install nmap`. Once it's installed, the goal is to search the machine for any open ports.


```shell

```



<p align="center">
  <img src="">
</p>
