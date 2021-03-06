# Nginx website with SSL
**Nginx configuration for using SSL with [filestash](https://www.filestash.app/) and [gitea](https://gitea.io/en-us/). Try it [here](https://r4v10l1.github.io/filestash-ssl/var/www/html/).**

The nginx folder is intended for hosting filestash under `filestash.YOUR-DOMAIN.com` and gitea under `git.YOUR-DOMAIN.com`.  
It will proxy all the trafic from:
```bash
http://YOUR-DOMAIN.com:80           -> https://YOUR-DOMAIN.com:443
http://filestash.YOUR-DOMAIN.com:80 -> https://filestash.YOUR-DOMAIN.com:443 -> https://YOUR-DOMAIN.com:8334  
http://git.YOUR-DOMAIN.com:80       -> https://git.YOUR-DOMAIN.com:443       -> https://YOUR-DOMAIN.com:3000  
```

### Configuration
You need to edit all `YOUR-DOMAIN` and all `YOUR-USER`. In my case I used a local IP or a random ass domain for the domain and it worked so you can try.
```c
./filestash/docker-compose.yml:7:           - /home/YOUR-USER/filestash/DATA:/app/data/state

./nginx/nginx.conf:30:                      server_name YOUR-DOMAIN.com www.YOUR-DOMAIN.com;
./nginx/nginx.conf:66:                      server_name YOUR-DOMAIN.com www.YOUR-DOMAIN.com;
./nginx/sites-available/filestash.conf:7:   server_name filestash.YOUR-DOMAIN.com;
./nginx/sites-available/git.conf:5:         server_name git.YOUR-DOMAIN.com;

./var/www/html/index.html:15:               <div class="text-title">Welcome to YOUR-DOMAIN.com&trade;</div>
./var/www/html/index.html:17:               <button onclick="window.location='https://filestash.YOUR-DOMAIN.com/'">Go to filestash</button>
```
You will also need to edit the `EDIT-ME` on the HTML.
```c
./var/www/html/404.html:8:                  <meta property="og:url" content="EDIT-ME - https://YOUR-DOMAIN.com">

./var/www/html/index.html:5:                <meta name="description" content="EDIT-ME - The description">
./var/www/html/index.html:6:                <meta property="og:title" content="EDIT-ME - Main page title">
./var/www/html/index.html:7:                <meta property="og:image" content="EDIT-ME - /img/favicon.png">
./var/www/html/index.html:8:                <meta property="og:url" content="EDIT-ME - https://YOUR-DOMAIN.com">
./var/www/html/index.html:9:                <meta property="og:description" content="EDIT-ME - The description (previews)">
./var/www/html/index.html:10:               <title>EDIT-ME - Your title</title>

./var/www/html/state.html:8:                <meta property="og:url" content="EDIT-ME - https://YOUR-DOMAIN.com/state">
./var/www/html/state.html:9:                <meta property="og:description" content="EDIT-ME - Current state of YOUR-DOMAIN.com">
```
Keep in mind that you will also need to add the **CNAME** to the domain:
```bash
CNAME  |  filestash.YOUR-DOMAIN.com  ->  YOUR-DOMAIN.com
CNAME  |        git.YOUR-DOMAIN.com  ->  YOUR-DOMAIN.com
```
