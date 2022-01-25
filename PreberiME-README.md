# Apache-2-4
Apache 2.4 Strani z napakami v Slovenskem jeziku / localized-error-pages-apache-modul Slovenian Language translate

Modul Localized Error Pages for Apache 2.4 - Slovenski prevod strani z napakami za vaš strežnik:

Postopek aktivacije in namestitve modula za strani z napakami:
## Namestitev Apache:
sudo apt update
sudo apt install apache2 unzip -y

## Aktivacija modulov: Negotiation + Include + Alias in po potrebi še Rewrite
Ko ste namestili Apache WEB Server aktivirajte v terminalu sledeče module:
sudo a2enmod negotiation include alias rewrite

## Zdaj v terminalu ali lažje vam je uporaba WEBMin modificirajte oz. odkomentirajte, kot je spodaj prikazano linijo 37-39 ter 44-79 ne pozavit vpisat v liniji 56 še slovensko kodo se pravi sl:
sudo nano /etc/apache2/conf-available/localized-error-pages.conf

# Customizable error responses come in three flavors:
# 1) plain text
# 2) local redirects
# 3) external redirects
#
# Some examples:
#ErrorDocument 500 "The server made a boo boo."
#ErrorDocument 404 /missing.html
#ErrorDocument 404 "/cgi-bin/missing_handler.pl"
#ErrorDocument 402 http://www.example.com/subscription_info.html
#

#
# Putting this all together, we can internationalize error responses.
#
# We use Alias to redirect any /error/HTTP_<error>.html.var response to
# our collection of by-error message multi-language collections.  We use
# includes to substitute the appropriate text.
#
# You can modify the messages' appearance without changing any of the
# default HTTP_<error>.html.var files by adding the line:
#
#Alias /error/include/ "/your/include/path/"
#
# which allows you to create your own set of files by starting with the
# /usr/share/apache2/error/include/ files and copying them to /your/include/path/,
# even on a per-VirtualHost basis.  If you include the Alias in the global server
# context, is has to come _before_ the 'Alias /error/ ...' line.
#
# The default include files will display your Apache version number and your
# ServerAdmin email address regardless of the setting of ServerSignature.
#
# WARNING: The configuration below will NOT work out of the box if you have a
#		  SetHandler directive in a <Location /> context somewhere. Adding
#		  the following three lines AFTER the <Location /> context should
#		  make it work in most cases:
<Location /error/>
    SetHandler none
</Location>
#
# The internationalized error documents require mod_alias, mod_include
# and mod_negotiation.  To activate them, uncomment the following 37 lines.

<IfModule mod_negotiation.c>
    <IfModule mod_include.c>
        <IfModule mod_alias.c>

            Alias /error/ "/usr/share/apache2/error/"

            <Directory "/usr/share/apache2/error">
                Options IncludesNoExec
                AddOutputFilter Includes html
                AddHandler type-map var
                Order allow,deny
                Allow from all
                LanguagePriority en sl cs de es fr it nl sv pt-br ro
                ForceLanguagePriority Prefer Fallback
            </Directory>

            ErrorDocument 400 /error/HTTP_BAD_REQUEST.html.var
            ErrorDocument 401 /error/HTTP_UNAUTHORIZED.html.var
            ErrorDocument 403 /error/HTTP_FORBIDDEN.html.var
            ErrorDocument 404 /error/HTTP_NOT_FOUND.html.var
            ErrorDocument 405 /error/HTTP_METHOD_NOT_ALLOWED.html.var
            ErrorDocument 408 /error/HTTP_REQUEST_TIME_OUT.html.var
            ErrorDocument 410 /error/HTTP_GONE.html.var
            ErrorDocument 411 /error/HTTP_LENGTH_REQUIRED.html.var
            ErrorDocument 412 /error/HTTP_PRECONDITION_FAILED.html.var
            ErrorDocument 413 /error/HTTP_REQUEST_ENTITY_TOO_LARGE.html.var
            ErrorDocument 414 /error/HTTP_REQUEST_URI_TOO_LARGE.html.var
            ErrorDocument 415 /error/HTTP_UNSUPPORTED_MEDIA_TYPE.html.var
            ErrorDocument 500 /error/HTTP_INTERNAL_SERVER_ERROR.html.var
            ErrorDocument 501 /error/HTTP_NOT_IMPLEMENTED.html.var
            ErrorDocument 502 /error/HTTP_BAD_GATEWAY.html.var
            ErrorDocument 503 /error/HTTP_SERVICE_UNAVAILABLE.html.var
            ErrorDocument 506 /error/HTTP_VARIANT_ALSO_VARIES.html.var
        </IfModule>
    </IfModule>
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet


## Naložite to datoteko in jo odpakirajte v vašo mapo "/usr/share/apache2/error/" datoteka "localized-error-pages-apache-modul.zip":
## Lahko si poberete to zip datoteko tukaj preko GitHub ali pa direktno iz mojega serveja se pravi tako:
cd /usr/share/apache2/error/
sudo wget https://https://piramide.zapto.org/localized-error-pages-apache-modul.zip
unzip localized-error-pages-apache-modul.zip
sudo systemctl restart apache2
  ali
sudo service apache2 restart
  
## Lažje vam bo če upravljate vaš strežnik preko WEBMin priporočljivo za lažje upravljanje in konfiguracijo datotek = https://webmin.com/
Naložite to datoteko preko github v vašo mapo "/usr/share/apache2/error/" ker jo z desnim gumbom miške ko kliknete na mapo zip odpakirate v meniju "Extract"
  Ponovno zaženite apache to bo vse!

Prikaz strani z napakami lahko modificirate v mapi "/usr/share/apache2/error/include/"
  Datoteke so: top.html + spacer.html + bottom.html
  
  Po želji pa si lahko generirate v vaši XY mapi željene streni z napakami, zdaj bo vaš Apache WEB Server produciral kode z napakami v vseh teh jehikih, se pravi na bazi uporabnikove lokacije oz. jezika spletnega brskalnika:
  en sl cs de es fr it nl sv pt-br ro

  
  
