<h1 align="center">Interactsh</h1>
<h4 align="center">An OOB interaction gathering server and client library</h4>


<p align="center">
<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-_red.svg"></a>
<a href="https://github.com/projectdiscovery/interactsh/issues"><img src="https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat"></a>
<a href="https://goreportcard.com/badge/github.com/projectdiscovery/interactsh"><img src="https://goreportcard.com/badge/github.com/projectdiscovery/interactsh"></a>
<a href="https://twitter.com/pdiscoveryio"><img src="https://img.shields.io/twitter/follow/pdiscoveryio.svg?logo=twitter"></a>
<a href="https://discord.gg/projectdiscovery"><img src="https://img.shields.io/discord/695645237418131507.svg?logo=discord"></a>
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#usage">Usage</a> •
  <a href="#interactsh-client">Interactsh Client</a> •
  <a href="#interactsh-server">Interactsh Server</a> •
  <a href="#interactsh-integration">Interactsh Integration</a> •
  <a href="https://discord.gg/projectdiscovery">Join Discord</a>
</p>

---

**Interactsh** is an Open-Source Solution for Out of band Data Extraction, A tool designed to detect bugs that cause external interactions, For example - Blind SQLi, Blind CMDi, SSRF, etc.


# Features

- DNS/HTTP(S)/SMTP(S)/LDAP Interaction support
- NTLM/SMB/FTP/RESPONDER Listener support **(self-hosted)**
- Wildcard Interaction support **(self-hosted)**
- CLI / Web / Burp / ZAP / Docker client support
- Self hosted Interactsh server support
- AES encryption with zero logging
- Automatic ACME based Wildcard TLS w/ Auto Renewal
- DNS Entries for Cloud Metadata service

# Interactsh Client

## Usage

```sh
interactsh-client -h
```

This will display help for the tool. Here are all the switches it supports.

```yaml
Usage:
  ./interactsh-client [flags]

Flags:
INPUT:
   -s, -server string  interactsh server(s) to use (default "oast.pro,oast.live,oast.site,oast.online,oast.fun,oast.me")

CONFIG:
   -n, -number int          number of interactsh payload to generate (default 1)
   -t, -token string        authentication token to connect protected interactsh server
   -pi, -poll-interval int  poll interval in seconds to pull interaction data (default 5)
   -nf, -no-http-fallback   disable http fallback registration
   -persist                 enables persistent interactsh sessions

FILTER:
   -dns-only   display only dns interaction in CLI output
   -http-only  display only http interaction in CLI output
   -smtp-only  display only smtp interactions in CLI output

OUTPUT:
   -o string  output file to write interaction data
   -json      write output in JSONL(ines) format
   -v         display verbose interaction
```

## Interactsh CLI Client

Interactsh Cli client requires **go1.17+** to install successfully. Run the following command to get the repo - 

```sh
go install -v github.com/projectdiscovery/interactsh/cmd/interactsh-client@latest
```

### Default Run

This will generate a unique payload that can be used for OOB testing with minimal interaction information in the ouput.

```console
interactsh-client

    _       __                       __       __  
   (_)___  / /____  _________ ______/ /______/ /_ 
  / / __ \/ __/ _ \/ ___/ __ '/ ___/ __/ ___/ __ \
 / / / / / /_/  __/ /  / /_/ / /__/ /_(__  ) / / /
/_/_/ /_/\__/\___/_/   \__,_/\___/\__/____/_/ /_/ v0.0.5

        projectdiscovery.io

[INF] Listing 1 payload for OOB Testing
[INF] c23b2la0kl1krjcrdj10cndmnioyyyyyn.oast.pro

[c23b2la0kl1krjcrdj10cndmnioyyyyyn] Received DNS interaction (A) from 172.253.226.100 at 2021-26-26 12:26
[c23b2la0kl1krjcrdj10cndmnioyyyyyn] Received DNS interaction (AAAA) from 32.3.34.129 at 2021-26-26 12:26
[c23b2la0kl1krjcrdj10cndmnioyyyyyn] Received HTTP interaction from 43.22.22.50 at 2021-26-26 12:26
[c23b2la0kl1krjcrdj10cndmnioyyyyyn] Received DNS interaction (MX) from 43.3.192.3 at 2021-26-26 12:26
[c23b2la0kl1krjcrdj10cndmnioyyyyyn] Received DNS interaction (TXT) from 74.32.183.135 at 2021-26-26 12:26
[c23b2la0kl1krjcrdj10cndmnioyyyyyn] Received SMTP interaction from 32.85.166.50 at 2021-26-26 12:26
```

### Verbose Mode


Running the `interactsh-client` in **verbose mode** (v) to see the whole request and response, along with an output file to analyze afterwards.

```console
interactsh-client -v -o interactsh-logs.txt

    _       __                       __       __  
   (_)___  / /____  _________ ______/ /______/ /_ 
  / / __ \/ __/ _ \/ ___/ __ '/ ___/ __/ ___/ __ \
 / / / / / /_/  __/ /  / /_/ / /__/ /_(__  ) / / /
/_/_/ /_/\__/\___/_/   \__,_/\___/\__/____/_/ /_/ v1.0.0

    projectdiscovery.io

[INF] Listing 1 payload for OOB Testing
[INF] c58bduhe008dovpvhvugcfemp9yyyyyyn.oast.pro

[c58bduhe008dovpvhvugcfemp9yyyyyyn] Received HTTP interaction from 103.22.142.211 at 2021-09-26 18:08:07
------------
HTTP Request
------------

GET /favicon.ico HTTP/2.0
Host: c58bduhe008dovpvhvugcfemp9yyyyyyn.oast.pro
Referer: https://c58bduhe008dovpvhvugcfemp9yyyyyyn.oast.pro
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.82 Safari/537.36


-------------
HTTP Response
-------------

HTTP/1.1 200 OK
Connection: close
Content-Type: text/html; charset=utf-8
Server: oast.pro

<html><head></head><body>nyyyyyy9pmefcguvhvpvod800ehudb85c</body></html>
```

### Using Self-Hosted server

Using the `server` flag, `interactsh-client` can be configured to connect with a self-hosted Interactsh server, this flag accepts single or multiple server separated by comma.

```sh
interactsh-client -server hackwithautomation.com
```

We maintain a list of default Interactsh servers to use with `interactsh-client`:

- oast.pro
- oast.live
- oast.site
- oast.online
- oast.fun
- oast.me

Default servers are subject to change/rotate/down at any time, thus we recommend using a self-hosted interactsh server if you are experiencing issues with the default server.

### Using Protected Self-Hosted server

Using the `token` flag, `interactsh-client` can connect to a self-hosted Interactsh server that is protected with authentication.

```sh
interactsh-client -server hackwithautomation.com -token XXX
```

### Using with Notify

If you are away from your terminal, you may use [notify](https://github.com/projectdiscovery/notify) to send a real-time interaction notification to any supported platform.

```sh
interactsh-client | notify
```

![image](https://user-images.githubusercontent.com/8293321/116283535-9bcac180-a7a9-11eb-94d5-0313d4812fef.png)


## Interactsh Web Client

[Interactsh-web](https://github.com/projectdiscovery/interactsh-web) is a free and open-source web client that displays Interactsh interactions in a well-managed dashboard in your browser. It uses the browser's local storage to store and display all incoming interactions. By default, the web client is configured to use - **interachsh.com**, a cloud-hosted interactsh server, and supports other self-hosted public/authencaited interactsh servers as well.

A hosted instance of **interactsh-web** client is available at https://app.interactsh.com

<img width="2032" alt="interactsh-web" src="https://user-images.githubusercontent.com/8293321/136621531-d72c9ece-0076-4db1-98c9-21dcba4ba09c.png">

## Interactsh Docker Client

A [Docker image](https://hub.docker.com/r/projectdiscovery/interactsh-client) is also provided with interactsh client that is ready to run and can be used in the following way:

```sh
docker run projectdiscovery/interactsh-client:latest
```

```console
docker run projectdiscovery/interactsh-client:latest

    _       __                       __       __  
   (_)___  / /____  _________ ______/ /______/ /_ 
  / / __ \/ __/ _ \/ ___/ __ '/ ___/ __/ ___/ __ \
 / / / / / /_/  __/ /  / /_/ / /__/ /_(__  ) / / /
/_/_/ /_/\__/\___/_/   \__,_/\___/\__/____/_/ /_/ v1.0.0

        projectdiscovery.io

[INF] Listing 1 payload for OOB Testing
[INF] c59e3crp82ke7bcnedq0cfjqdpeyyyyyn.oast.pro
```

## Burp Suite Extension

[interactsh-collaborator](https://github.com/wdahlenburg/interactsh-collaborator) is Burp Suite extension developed and maintained by [@wdahlenb](https://twitter.com/wdahlenb)

- Download latest JAR file from [releases](https://github.com/wdahlenburg/interactsh-collaborator/releases) page.
- Open Burp Suite &rarr; Extender &rarr; Add &rarr; Java &rarr; Select JAR file &rarr; Next
- New tab named **Interactsh** will be appeared upon successful installation.
- See the [interactsh-collaborator](https://github.com/wdahlenburg/interactsh-collaborator) project for more info.

<img width="2032" alt="burp" src="https://user-images.githubusercontent.com/8293321/135176099-0e3fa01c-bdce-4f04-a94f-de0a34c7abf6.png">

## OWASP ZAP Add-On

Interactsh can be used with OWASP ZAP via the [OAST add-on for ZAP](https://www.zaproxy.org/docs/desktop/addons/oast-support/). With ZAP's scripting capabilities, you can create powerful out-of-band scan rules that leverage Interactsh's features. A standalone script template has been provided as an example (it is added automatically when you install the add-on).

- Install the OAST add-on from the [ZAP Marketplace](https://www.zaproxy.org/addons/).
- Go to Tools &rarr; Options &rarr; OAST and select **Interactsh**.
- Configure [the options](https://www.zaproxy.org/docs/desktop/addons/oast-support/services/interactsh/options/) for the client and click on "New Payload" to generate a new payload.
- OOB interactions will appear in the [OAST Tab](https://www.zaproxy.org/docs/desktop/addons/oast-support/tab/) and you can click on any of them to view the full request and response.
- See the [OAST add-on documentation](https://www.zaproxy.org/docs/desktop/addons/oast-support/) for more info.

![zap](https://user-images.githubusercontent.com/16446369/135211920-ed24ba5a-5547-4cd4-b6d8-656af9592c20.png)

-------


# Interactsh Server

Interactsh server runs multiple services and captures all the incoming requests. To host an instance of interactsh-server, you are required to have the follow requirements:

1. Domain name with custom **host names** and **nameservers**.
2. Basic droplet running 24/7 in the background.

# Usage

```sh
interactsh-server -h
```

This will display help for the tool. Here are all the switches it supports.

```yaml
Usage:
  ./interactsh-server [flags]

Flags:
INPUT:
   -d, -domain string       configured domain to use with interactsh server
   -ip string               public ip address to use for interactsh server
   -lip, -listen-ip string  public ip address to listen on (default "0.0.0.0")
   -e, -eviction int        number of days to persist interaction data in memory (default 30)
   -a, -auth                enable authentication to server using random generated token
   -t, -token string        enable authentication to server using given token
   -acao-url string         origin url to send in acao header (required to use web-client) (default "https://app.interactsh.com")
   -sa, -skip-acme          skip acme registration (certificate checks/handshake + TLS protocols will be disabled)

SERVICES:
   -dns-port int           port to use for dns service (default 53)
   -http-port int          port to use for http service (default 80)
   -https-port int         port to use for https service (default 443)
   -smtp-port int          port to use for smtp service (default 25)
   -smtps-port int         port to use for smtps service (default 587)
   -smtp-autotls-port int  port to use for smtps autotls service (default 465)
   -ldap-port int          port to use for ldap service (default 389)
   -ldap                   enable ldap server with full logging (authenticated)
   -wc, -wildcard          enable wildcard interaction for interactsh domain (authenticated)
   -smb                    start smb agent - impacket and python 3 must be installed (authenticated)
   -responder              start responder agent - docker must be installed (authenticated)
   -ftp                    start ftp agent (authenticated)
   -smb-port int           port to use for smb service (default 445)
   -ftp-port int           port to use for ftp service (default 21)
   -ftp-dir string         ftp directory - temporary if not specified

DEBUG:
   -debug  start interactsh server in debug mode
```

We are using GoDaddy for domain name and DigitalOcean droplet for the server, a basic $5 droplet should be sufficient to run self-hosted Interactsh server. If you are not using GoDaddy, follow your registrar's process for creating / updating DNS entries.

<table>
<td>

## Configuring Interactsh domain

- Navigate to `https://dcc.godaddy.com/manage/{{domain}}/dns/hosts`
- Advanced Features &rarr; Host names &rarr; Add &rarr; Submit `ns1`, `ns2` with your `SERVER_IP` as value

<img width="1288" alt="gdd-hostname" src="https://user-images.githubusercontent.com/8293321/135175512-135259fb-0490-4038-845a-0b62b1b8f549.png">

- Navigate to `https://dns.godaddy.com/{{domain}}/nameservers`
- I'll use my own nameservers &rarr; Submit `ns1.INTERACTSH_DOMAIN`, `ns2.INTERACTSH_DOMAIN`

<img width="1288" alt="gdd-ns" src="https://user-images.githubusercontent.com/8293321/135175627-ea9639fd-353d-441b-a9a4-dae7f540d0ae.png">

</td>
</table>

<table>
<td>

## Configuring Interactsh server

Install `interactsh-server` on your **VPS**

```bash
go install -v github.com/projectdiscovery/interactsh/cmd/interactsh-server@latest
```

Considering domain name setup is **completed**, run the below command to run `interactsh-server`

```bash
interactsh-server -domain INTERACTSH_DOMAIN
```

Following is an example of a successful installation and operation of a self-hosted server:

![interactsh-server](https://user-images.githubusercontent.com/8293321/134819391-51137c77-61d5-4ae8-9504-947dd444d863.png)


A number of needed flags are configured automatically to run `interactsh-server` with default settings. For example, `ip` and `listen-ip` flags set with the Public IP address of the system when possible.

</td>
</table>

## Running Interactsh Server

```console
interactsh-server -domain interact.sh

    _       __                       __       __  
   (_)___  / /____  _________ ______/ /______/ /_ 
  / / __ \/ __/ _ \/ ___/ __ '/ ___/ __/ ___/ __ \
 / / / / / /_/  __/ /  / /_/ / /__/ /_(__  ) / / /
/_/_/ /_/\__/\___/_/   \__,_/\___/\__/____/_/ /_/ v1.0.0

                projectdiscovery.io

[INF] Listening with the following services:
[HTTPS] Listening on TCP 46.101.25.250:443
[HTTP] Listening on TCP 46.101.25.250:80
[SMTPS] Listening on TCP 46.101.25.250:587
[LDAP] Listening on TCP 46.101.25.250:389
[SMTP] Listening on TCP 46.101.25.250:25
[DNS] Listening on TCP 46.101.25.250:53
[DNS] Listening on UDP 46.101.25.250:53
```

There are more useful capabilities supported by `interactsh-server` that are not enabled by default and are intended to be used only by **self-hosted** servers.

## Wildcard Interaction

To enable `wildcard` interaction for configured Interactsh domain `wildcard` flag can be used with implicit authentication protection via the `auth` flag if the `token` flag is omitted.

```console
interactsh-server -domain hackwithautomation.com -wildcard

    _       __                       __       __  
   (_)___  / /____  _________ ______/ /______/ /_ 
  / / __ \/ __/ _ \/ ___/ __ '/ ___/ __/ ___/ __ \
 / / / / / /_/  __/ /  / /_/ / /__/ /_(__  ) / / /
/_/_/ /_/\__/\___/_/   \__,_/\___/\__/____/_/ /_/ v1.0.0

        projectdiscovery.io

[INF] Client Token: 699c55544ce1604c63edb769e51190acaad1f239589a35671ccabd664385cfc7
[INF] Listening with the following services:
[HTTPS] Listening on TCP 157.230.223.165:443
[HTTP] Listening on TCP 157.230.223.165:80
[SMTPS] Listening on TCP 157.230.223.165:587
[LDAP] Listening on TCP 157.230.223.165:389
[SMTP] Listening on TCP 157.230.223.165:25
[DNS] Listening on TCP 157.230.223.165:53
[DNS] Listening on UDP 157.230.223.165:53
```

# Interactsh Integration

### Nuclei - OAST

[Nuclei](https://github.com/projectdiscovery/nuclei) vulnerability scanner utilize **Interactsh** for automated payload generation and detection of out of band based security vulnerabilities.

See [Nuclei + Interactsh](https://blog.projectdiscovery.io/nuclei-interactsh-integration/) Integration blog and [guide document](https://nuclei.projectdiscovery.io/templating-guide/interactsh/) for more information.

# Cloud Metadata

Interactsh server supports DNS records for cloud metadata services, which is useful for testing SSRF-related vulnerabilities.

Currently supported metadata services:

- [AWS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)
- [Alibaba](https://www.alibabacloud.com/blog/alibaba-cloud-ecs-metadata-user-data-and-dynamic-data_594351)

Example:

* **aws.interact.sh** points to 169.254.169.254
* **alibaba.interact.sh** points to 100.100.100.200

-----

### Acknowledgement

Interactsh is inspired from [Burp Collaborator](https://portswigger.net/burp/documentation/collaborator).

### License

Interactsh is distributed under [MIT License](https://github.com/projectdiscovery/interactsh/blob/master/LICENSE.md) and made with 🖤 by the [projectdiscovery](https://projectdiscovery.io) team.