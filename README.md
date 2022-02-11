# SCPrime Monitor

![In Development](https://img.shields.io/badge/Development%20Stage-Early%20Access-yellow.svg?style=flat)
![In Development](https://img.shields.io/badge/Requirements-Docker-green.svg?style=flat)

## Installation Wizard
You can either use [Portainer](https://www.portainer.io) or have Docker and Docker Compose 
installed on the system where you wish to run this application.

Once you have the met all the requirements either use this repository to create a 'stack' for 
your containers or copy and pate the script in the comments below.

```yaml
version: '3.4'

services:
  database:
    image: mysql
    container_name: MySQL
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: scprime@Mon2022!
    networks:
      scpm:
    volumes:
      - database_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin" ,"ping", "-h", "localhost"]
      timeout: 20s
      retries: 10

  monitor:
    depends_on:
      database:
        condition: service_healthy
    image: addramyr/scp-monitor:latest
    container_name: SCPrimeMonitor
    restart: always
    ports:
      - 8016:80
      - 1932:1932
    networks:
      scpm:

networks:
  scpm:
    name: "SCPMonitorNetwork"
    driver: bridge

volumes:
  database_data:
```

## Initial Login
Once SCPrime Monitor has been set up and the database installed you can log in using the
following initial credentials:
- **Username:** admin@domain.com
- **Password:** qwertyuiop

If this is the fist time logging in it will ask you to enter your new credentials to configure
your account on the system.

## Compatability
Below is a table of the current host (storage provider) compatability list.

| Operating System | Status                                                                | Plans        |
|------------------|-----------------------------------------------------------------------|--------------|
| DIY Linux        | Fully Compatible                                                      |              |
| DIY Windows      | Fully Compatible                                                      |              |
| DIY Docker       | Can use the linux option if the container is running on same network  | Coming Soon  |
| DIY Apple        | No support officially, this is due to lack of interest on the network | Coming Later |
| XA Miner         | Currently no way of communicating with the hosts API                  | Coming Soon  |

## Known Issues
- Currently, there is no support for any other SSH authentication except password. This will be 
patched in an upcoming update where we will introduce SSH Key Pairs

## Help
If anyone has issues with the system please feel free to contact Addramyr#4310 on Discord, I am 
usually in [SCPrime Discord](https://discord.gg/scprime) or [SCPrime Monitor Discord](https://discord.gg/5mmg7C3MJG).

## ☕ Support
If you want to help support the project, that's great! I drink lots of coffee ☕.

You can tip using the software or by sending directly to my SCP wallet at 966125bc81f5200724a79c0862a8e8b61ed799d124b30d2ec5bc01f3dcabaefb7fc44a642d8d