# furao.link

furao.link is a dead simple link shortener written with in C++ using the [Crow](https://github.com/CrowCpp/Crow) web framework, and a [fork of NGN's Kisalt](https://github.com/ngn13/kisalt)..

### Deploy
First off, clone the repository. Make changes to the interface if needed.

CD into it.

Build your own Docker image with:
```docker build --tag 'furao' .```

Don't forget the dot at the end.

Then, you do this when it has been built.

```bash
docker run -d --restart=unless-stopped \
     -p 3402:8080            \
     -e URL=https://httpsurlhere.com      \
     -v $PWD/data:/data                \
     furao
```

Change the `URL` accordingly. Make sure it's https. You can also replace the 3402 with any available port you like.

To disable saving the links, you can use the `NOSAVE` option.
A volume is not needed when using this option:
```bash
docker run -d --restart=unless-stopped \
    -p 3402:8080             \
    -e URL=https://httpsurlhere.com       \
    -e NOSAVE=1                        \
    furao
```

### Placing behind reverse proxy

What the hell is NGINX? Just install Caddy.

```bash
apt-get install caddy
```

Then, make a little directory for your Caddyfile. It does not have to be in the same directory as furao-link.

```bash
mkdir caddyfiles
cd caddyfiles
```

Now, make a caddyfile. It must be called Caddyfile. Case-sensitive.

```bash
nano Caddyfile
```

Type this with the url you specificed (the domain must have the VPS/system IP linked to it, and, again, replace 3402 accordingly:

```bash
httpsurlhere.com {
    reverse_proxy localhost:3402
}```

Now, write out the file, and then type...

```bash
caddy reload```

Bam! You won the game. Now CD out and go ahead with your business.

### Usage
You can use the web interface to shorten links, or you can directly use the API:
```bash
curl https://k.example.com/add\?url=<url>```
