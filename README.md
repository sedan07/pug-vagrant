# PUG Vagrant

Vagrant boxes (using Docker containers) for Puppet Trusted Facts demo

**Important:** You need to clone the [pug-puppet](https://github.com/sedan07/pug-puppet) repo to some where on your machine first!

## Usage

Copy the `config.yaml.dist` file to `config.yaml` and replace the location of the puppet repo with the absolute path to it on your file system then:

Firstly spin up the `puppet-master` as this is used by the other two nodes

```
vagrant up pug-puppet-master
```

then spin up the other two:

```
vagrant up pug-web-http
vagrant up pug-web-https
``` 

Instead of SSH'ing into them you simply use the `docker exec` command, like so:

```
docker exec -it pug-web-https /bin/bash
```

The two web nodes automatically know the IP of the Puppet Master so you can simply run:

```
puppet agent -t
```

to kick off a Puppet run and experiment from there

## Port forwarding

http://localhost:80/ should point at the pug-web-http server
https://localhost:443/ should point at the pug-web-https server
