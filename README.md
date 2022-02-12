# SCPrime Monitor

![In Development](https://img.shields.io/badge/Current%20Version-1.10-blue.svg?style=flat)
![In Development](https://img.shields.io/badge/Development%20Stage-Early%20Access-yellow.svg?style=flat)
![In Development](https://img.shields.io/badge/Requirements-Docker-green.svg?style=flat)

### Docker Hub Location
https://hub.docker.com/r/addramyr/scp-monitor

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

## Host/Provider System Requirements:
### DIY Linux
| Software | Usage                                                                |
|----------|----------------------------------------------------------------------|
| OpenSSH  | Allows connection to server if using SSH. (SSH Default port is `22`) |
| CURL     | CURL allows for api requests to be sent to the host.                 |
### DIY Windows
| Software | Usage                                                                | Software Location                                                                               |
|----------|----------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| OpenSSH  | Allows connection to server if using SSH. (SSH Default port is `22`) | https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse |
| CURL     | CURL allows for api requests to be sent to the host.                 | https://curl.haxx.se/download.html                                                              |

Please Note: If using Windows as your host/provider you will need to configure openSSH as it not
configured or installed.

## System Ports
The system requires access to port `8016` (this can be changed) however is the default port for accessing
the main user interface.

Port `1932` is also a required port and at this point can not be changed as is used to communicate
with the server/api part of the monitor tool.

## Docker Recommended Port Configuration
Please **_do not change internal ports or external port `1932`_**, as this will break the system.

| Docker Layout | Usage                                             | External | Internal |
|---------------|---------------------------------------------------|----------|----------|
| 8016:80       | Main web interface (external port can be changed) | 8016     | 80       |
| 1932:1932     | Backend communication (do not change)             | 1932     | 1932     |

## Initial Login
Once SCPrime Monitor has been set up and the database installed you can log in using the
following initial credentials:
- **Username:** admin@domain.com
- **Password:** qwertyuiop

If this is the fist time logging in it will ask you to enter your new credentials to configure
your account on the system.

## Compatability
Below is a table of the current host (storage provider) compatability list.

| Operating System | Status                                                                          | Plans       |
|------------------|---------------------------------------------------------------------------------|-------------|
| DIY Linux        | Fully Compatible                                                                |             |
| DIY Windows      | Fully Compatible                                                                |             |
| DIY Docker       | Can use the linux option if the container is running on same network (untested) | Coming Soon |
| DIY Apple        | Lightly tested using SSH                                                        |             |
| XA Miner         | Currently no way of communicating with the hosts API                            | Coming Soon |

## Known Issues
- Currently, there is no support for any other SSH authentication except password. This will be 
patched in an upcoming update where we will introduce SSH Key Pairs.

## Change Log

### 1.10

#### New Features
- Added ability to add custom wallet addresses to your account. These are not related to any given host and allow viewing of that wallets transaction history. Useful if wanting to know the status of offline wallets like cold ones.
- Added change log.

#### Changed Features
- Increased error message time out from 2 seconds to 10 seconds.
- Updated server to allow apple connections.
- Locked down Storage Per TB/Month and Collateral Per TB/Month to numbers between 1 and 5 SCP.

#### Fixed Bugs
- Fixed an issue that caused the Auto Announce system to get stuck and never run again.
- Fixed a bug in the admin area preventing users from being displayed.
- Background tasks were getting stuck after initial execution.
- Investigating a connection issue with Ubuntu Desktop (20.04).
- Fixed a bug on the currency conversion calculator that makes the system think 1 Hash = 1 SCP.

#### General Cleanup
- Removed a few debug lines on the server logs when certain events fire whilst the system is active.

## Help
If anyone has issues with the system please feel free to contact Addramyr#4310 on Discord, I am 
usually in [SCPrime Discord](https://discord.gg/scprime) or [SCPrime Monitor Discord](https://discord.gg/5mmg7C3MJG).

## ☕ Support
If you want to help support the project, that's great! I drink lots of coffee ☕.

You can tip using the software or by sending directly to my SCP wallet at 966125bc81f5200724a79c0862a8e8b61ed799d124b30d2ec5bc01f3dcabaefb7fc44a642d8d