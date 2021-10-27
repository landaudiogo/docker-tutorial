> Docker Installation link:
> https://docs.docker.com/desktop/windows/install/

Which will direct you to this link
> Windows documentation to install wsl2:
> https://docs.microsoft.com/en-us/windows/wsl/install

Open up powershell and run the following commands: 
```
wsl --install -d Ubuntu
```

After this step ubuntu will open for you and you just have to set up the unix user for you.
```
wsl --set-default Ubuntu
wsl --set-version Ubuntu 2
```

Download the docker package for windows and install

open docker desktop
go to > settings > resources > wsl integration
and select the distro you want to activate which in our case is Ubuntu if it isn't activated

Within ubuntu:
docker --version
and check whether ubuntu has docker installed

Within ubuntu:
```
sudo docker run hello-world
```
If the installation was performed correctly the output of ths command will indicate it.

Docker desktop for windows already includes docker-compose, so there will be no need to install this package.
As a sanity check, run: 
```
docker-compose --version
```