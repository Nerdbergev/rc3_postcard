# RC3 Nowhere - Postcardwall
Quick and dirty...
## Install
```
python -m venv .venv
./.venv/bin/activate
pip install -r requirements.txt
```
Copy config.sample.ini to config.ini and edit it.

## Run

### Apache2
```
<IfModule mod_ssl.c>
<VirtualHost *:443>
        ServerName postcard.nerdberg.de
        Alias /static "/opt/rc3_postcardwall/static"
        WSGIDaemonProcess postcard threads=5 python-home=/opt/rc3_postcardwall/venv python-path=/opt/rc3_postcardwall/venv/lib/python3.8/site-packages
        WSGIScriptAlias / "/opt/rc3_postcardwall/postcard.wsgi"

        <Directory /opt/rc3_postcardwall>
                WSGIProcessGroup postcard
                WSGIApplicationGroup %{GLOBAL}
                Require all granted
        </Directory>

Include /etc/letsencrypt/options-ssl-apache.conf
SSLCertificateFile /etc/letsencrypt/live/lists.nerdberg.de/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/lists.nerdberg.de/privkey.pem
</VirtualHost>
</IfModule>
```

### Cronjob
```
*/2 * * * * PYTHON_HOME=/opt/rc3_postcardwall/venv/lib/python3.10/site-packages/ /opt/rc3_postcardwall/venv/bin/python3 /opt/rc3_postcardwall/imap.py >/dev/null
```
