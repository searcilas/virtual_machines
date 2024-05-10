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

## Scanning open ports

To do this let's use Nmap. If your Linux distribution has not nmap installed don't worry. Instal it using the command `sudo apt install nmap`. Once it's installed, the goal is to search the machine for any open ports.
```shell
sudo nmap -p- -sVC -sC --open -sS -vvv -n -Pn 172.17.0.2 
```
`-p-` open ports, `-sVC` service version running on those ports, `-sC`, `--open` only opened ports, `-sS`, `-vvv` verbose mode, `-n` no DNS resolution, `-Pn`y

![image](https://github.com/searcilas/virtual_machines/assets/105607232/912a5cb1-ebe9-4f12-b70d-81a32f4661ef)

As we can see, port 80 is open. It means it accepts connections over the network. More specifically, this port is used for HTTP web traffic. Port 22 is also open. It means the machine allows remote connection for command execution or file transferring.

Let's go to our web browser and see what we find :D

![image](https://github.com/searcilas/virtual_machines/assets/105607232/3a07b87e-3974-4ef0-8da7-3d13657246c9)

Apparently, it looks like an Apache2 web page. `Ctrl + U` to see if there's anything in the HTML code we can leverage.

Since we didn't find anything, let's try web fuzzing to see if there are any path segments. Since I'm currently on Linux Mint I'll the `wfuzz` tool. (The wordlist used is [onelistforallmicro.txt](https://github.com/six2dez/OneListForAll/blob/main/onelistforallmicro.txt))

```shell
wfuzz -c --sc 200 -t 200 -w onelistforallmicro.txt -u http://172.17.0.2/FUZZ
```
![image](https://github.com/searcilas/virtual_machines/assets/105607232/10163d10-ca80-4686-b028-02537eaed3d1)

Something interesting has been found: a path called `secret.php`. Let's take a peek in our browser.

![image](https://github.com/searcilas/virtual_machines/assets/105607232/f3e2f898-1bba-4dd2-b281-33d76b45926d)

We got a name. It's possible that it's a username used somewhere in the machine. If we backtrack a bit, let's remember port 22 was also open. Why don't we try an SSH connection using that username?

Cnnecting via SSH (using `ssh` command) requires a password. The issue is we don't know that password. In that case, let's consider a brute-force attack. Hydra is a powerful brute-force attack tool capable of being used on various protocols and services. In this case, the SSH protocol will be our guess.

```shell
hydra -l mario -P rockyou.txt 172.17.0.2 -t 64 ssh
```
![image](https://github.com/searcilas/virtual_machines/assets/105607232/cae36ed1-70dc-498f-9297-5a5121f7153f)

## Accesing the system

The results revealed the password "chocolate". Now, let's try the SSH connection with the `ssh` command.

```shell
ssh mario@172.17.0.2
```
![image](https://github.com/searcilas/virtual_machines/assets/105607232/570b777b-b486-45c9-8e77-1e8cde9b3265)

The goal is to become sudo user. With `sudo -l` it's possible to see a list of commands or programs that the user has permission to execute with sudo. (`find / -perm -4000 2>dev/null` también nos permite ver binarios ejecutándolos como propietarios del sistema)

![image](https://github.com/searcilas/virtual_machines/assets/105607232/d88221f8-eea7-4a58-9e53-b473195c380c)



<p align="center">
  <img src="">
</p>
