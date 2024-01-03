# Phpmyadmin-install

1. Aktualisieren Sie nun Ihre Paketlisten mit dem Befehl
   ```
   apt update
   ```


3. Installieren Sie jetzt möglicherweise verfügbare Updates der auf Ihrem Server bereits installieren Pakete mit dem Befehl
   ```
   apt upgrade -y
   ```


4. Als nächstes installieren Sie Pakete, die für die weiteren Installationen benötigt werden, mit folgendem Befehl:
   ```
   apt install ca-certificates apt-transport-https lsb-release gnupg curl nano unzip -y
   ```


Für Debian:
Fügen Sie mithilfe des Befehls
```
curl -fsSL https://packages.sury.org/php/apt.gpg -o /usr/share/keyrings/php-archive-keyring.gpg
```
den für die PHP-Paketquelle benötigen Key hinzu.

Verwenden Sie den Befehl 
```
echo "deb [signed-by=/usr/share/keyrings/php-archive-keyring.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
```
um nun die Paketquelle hinzuzufügen.


5. Aktualisieren Sie nun erneut Ihre Paketlisten mit dem Befehl
   ```
   apt update
   ```


6. Installieren Sie den Apache2-Webserver sowie weitere benötigte Pakete mit folgendem Befehl:
   ```
   apt install apache2 -y
   ```


7. Installieren Sie anschließend PHP 8 sowie einige wichtige PHP-Module. Der Befehl hierfür lautet:
    ```
   apt install php8.2 php8.2-cli php8.2-common php8.2-curl php8.2-gd php8.2-intl php8.2-mbstring php8.2-mysql php8.2-opcache php8.2-readline php8.2-xml php8.2-xsl php8.2-zip php8.2-bz2 libapache2-mod-php8.2 -y
    ```


8. Als nächstes installieren Sie den MariaDB-Server und -Client (MySQL) mit dem Befehl
```
apt install mariadb-server mariadb-client -y
```


9. Schließen Sie nun die Konfiguration des MariaDB-Servers ab:
Debian 11:

Geben Sie nun den Befehl
```
mysql_secure_installation
```
ein. Bei der ersten Abfrage des aktuellen Passworts müssen Sie nichts eingeben, sondern einfach die Enter-Taste drücken. Geben Sie bei der anschließenden Frage bzgl. des Wechsels zur Unix-Socket-Authentifizierung "n" ein und drücken Sie die Enter-Taste. Bestätigen Sie die nächste Frage bzgl. der Änderung des Root-Passworts mit Enter. Nun müssen Sie ein Passwort für den Root-Benutzer des MariaDB-Servers vergeben. Während der Eingabe erscheinen keine Zeichen, das ist jedoch normal. Bestätigen Sie alle darauffolgenden Fragen (Löschung des anonymen Benutzers, Verbieten des externen Root-Logins aus Sicherheitsgründen, Entfernen der Testdatenbank und Aktualisieren der Rechte) ebenfalls mit Enter. Danach ist der MariaDB-Server fertig installiert und konfiguriert.


10. Wechseln Sie mit dem Befehl
```
cd /usr/share
```
in das Verzeichnis, in dem phpMyAdmin installiert wird.


11. Um phpMyAdmin herunterzuladen, führen Sie nun den Befehl
```
wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.zip -O phpmyadmin.zip
```
aus


12. Entpacken Sie das soeben heruntergeladene Archiv mit dem folgenden Befehl:
```
unzip phpmyadmin.zip
```


13. Entfernen Sie das heruntergeladene Archiv, welches nun bereits entpackt ist, mit dem Befehl
```
rm phpmyadmin.zip
```


14. Anschließend müssen Sie den Namen des entpackten Verzeichnisses zu "phpmyadmin" umbenennen. Dies machen Sie mit folgendem Befehl:
```
mv phpMyAdmin-*-all-languages phpmyadmin
```


15. Vergeben Sie anschließend die benötigten Rechte auf das phpMyAdmin-Verzeichnis mithilfe des Befehls
```
chmod -R 0755 phpmyadmin
```


16. Erstellen Sie nun eine Apache2-Konfigurationsdatei für phpMyAdmin, indem Sie den Befehl
```
nano /etc/apache2/conf-available/phpmyadmin.conf
```
ausführen


17. Fügen Sie in diese Konfigurationsdatei nun folgenden Inhalt ein:
```
# phpMyAdmin Apache configuration

Alias /phpmyadmin /usr/share/phpmyadmin

<Directory /usr/share/phpmyadmin>
    Options SymLinksIfOwnerMatch
    DirectoryIndex index.php
</Directory>

# Disallow web access to directories that don't need it
<Directory /usr/share/phpmyadmin/templates>
    Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/libraries>
    Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/setup/lib>
    Require all denied
</Directory>
```
Speichern Sie Ihre Änderungen der Konfiguration, indem Sie STRG + X, danach die "Y"-Taste und anschließend Enter drücken.


18. Aktivieren Sie die soeben hinzugefügte Apache2-Konfigurationsdatei mit dem Befehl
```
a2enconf phpmyadmin
``` 
und führen daraufhin den Befehl
```
systemctl reload apache2
```
zum Neuladen des Apache2-Webservers aus.


19. Erstellen Sie das temporäre Verzeichnis, welches phpMyAdmin benötigt, indem Sie den Befehl
```
mkdir /usr/share/phpmyadmin/tmp/
```
ausführen.


20. Geben Sie dem Webserver-Benutzer nun die benötigten Besitzerrechte für dieses temporäre Verzeichnis mithilfe des Befehls
```
chown -R www-data:www-data /usr/share/phpmyadmin/tmp/
```


21. Beenden Sie die MariaDB-Konsole abschließend mit dem Befehl
```
exit
```
