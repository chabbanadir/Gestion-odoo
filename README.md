# Odoo For coffe shops 
This is odoo 14 with custom addons for coffe shops


# Quick install

Installing Odoo 14 with one command.

(Supports multiple Odoo instances on one server)

Install 
 [docker](https://docs.docker.com/get-docker/) 
 [docker-compose](https://docs.docker.com/compose/install/) yourself, 
 
then run:

``` bash
curl -s https://raw.githubusercontent.com/chabbanadir/Gestion-odoo/master/run.sh | sudo bash -s odoo-one 10014 20014
```

to set up first Odoo instance @ `localhost:10014` (default master password: `8sW8t2wFAgxC3dpk`)

and

``` bash
curl -s https://raw.githubusercontent.com/chabbanadir/Gestion-odoo/master/run.sh | sudo bash -s odoo-two 11014 21014
```

to set up another Odoo instance @ `localhost:11014` (default master password: `8sW8t2wFAgxC3dpk`)

Some arguments:
* First argument (**odoo-one**): Odoo deploy folder
* Second argument (**10014**): Odoo port
* Third argument (**20014**): live chat port

If `curl` is not found, install it:

``` bash
$ sudo apt-get install curl
# or
$ sudo yum install curl
```

# Usage

Start the container:
``` sh
docker-compose up
```

* Then open `localhost:10014` to access Odoo 12.0. If you want to start the server with a different port, change **10014** to another value in **docker-compose.yml**:

```
ports:
 - "10014:8069"
```

Run Odoo container in detached mode (be able to close terminal without stopping Odoo):

```
docker-compose up -d
```

**If you get the permission issue**, change the folder permission to make sure that the container is able to access the directory:

``` sh
$ git clone https://github.com/chabbanadir/Gestion-odoo
$ sudo chmod -R 777 addons
$ sudo chmod -R 777 etc
$ mkdir -p postgresql
$ sudo chmod -R 777 postgresql
```

Increase maximum number of files watching from 8192 (default) to **524288**. In order to avoid error when we run multiple Odoo instances. This is an *optional step*. These commands are for Ubuntu user:

```
$ if grep -qF "fs.inotify.max_user_watches" /etc/sysctl.conf; then echo $(grep -F "fs.inotify.max_user_watches" /etc/sysctl.conf); else echo "fs.inotify.max_user_watches = 524288" | sudo tee -a /etc/sysctl.conf; fi
$ sudo sysctl -p    # apply new config immediately
```

# Custom addons

The **addons/** folder contains custom addons. 
* IP Network Printer Drivers (ESCPOS) for POS Printing
* Control Discount & Price For the POS.
* Users/Cashiers can unlock POS screen by scanning their barcode or using security PIN
* Confirmation any action by user of specific group


# Odoo configuration & log

* To change Odoo configuration, edit file: **etc/odoo.conf**.
* Log file: **etc/odoo-server.log**
* Default database password (**admin_passwd**) is `8sW8t2wFAgxC3dpk`, please change it @ [etc/odoo.conf#L60](/etc/odoo.conf#L60)

# Odoo container management

**Run Odoo**:

``` bash
docker-compose up -d
```

**Restart Odoo**:

``` bash
docker-compose restart
```

**Stop Odoo**:

``` bash
docker-compose down
```

# Live chat

In [docker-compose.yml#L21](docker-compose.yml#L21), we exposed port **20014** for live-chat on host.

Configuring **nginx** to activate live chat feature (in production):

``` conf
#...
server {
    #...
    location /longpolling/ {
        proxy_pass http://0.0.0.0:20014/longpolling/;
    }
    #...
}
#...
```

# docker-compose.yml

* odoo:14.0
* postgres:13

## Odoo Coffe Applications 
![image](https://user-images.githubusercontent.com/67875720/176213106-ef6a4c14-3cea-404d-b1f7-ae9a76912840.png)

## POS (Point Of Sales)
![image](https://user-images.githubusercontent.com/67875720/176213443-08e8854e-13c8-4d59-bf55-b86401d0b2e2.png)

![image](https://user-images.githubusercontent.com/67875720/176213550-40f7e542-9b26-41fb-9880-1101866900db.png)

## Gestion d'achat 
![image](https://user-images.githubusercontent.com/67875720/176213679-03982245-9d6e-4e77-ab0d-b1a0d0035703.png)

## inventaire 

![image](https://user-images.githubusercontent.com/67875720/176214138-1a4e14e7-c569-4a8d-821d-a5c070f2f564.png)

## Production 

![image](https://user-images.githubusercontent.com/67875720/176214275-e9c73242-186f-423a-9b36-1e07e722b098.png)


