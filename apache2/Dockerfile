# apache2/Dockerfile
FROM httpd:2.4

# Copie de la configuration Apache
COPY apache.conf /usr/local/apache2/conf/httpd.conf

# Exposition du port HTTP
EXPOSE 80

# Commande par défaut pour démarrer Apache
CMD ["httpd-foreground"]
