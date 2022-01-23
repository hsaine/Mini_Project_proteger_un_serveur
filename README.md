<h1 align="center">Mini Project:proteger un serveur</h1>
                 
<h1 align="center">Objectif:</h1>

L’objectif de ce mini projet est de fournir un outil capable de protéger un serveur
contre les attaques abusives sur Internet, par bloquer automatiquement les adresses IP
malveillantes.

<h1 align="center"> Ressources:</h1>

AbuseIPDB est un projet dédié à la lutte contre la propagation des pirates, des
spammeurs et des activités abusives sur Internet. Il s’agit d’un service en ligne qui fournit
une liste des adresses IP ayant été marquées comme abusives via un API.

<h1 align="center"> Travail à réaliser:</h1>

Ce projet consiste à réaliser un script shell qui:<br>
➔ Fait l’extraction des adresses IP à partir des fichiers log (Apache, Nginx, SSH, etc).<br>
➔ Compare cette liste d’adresses IP avec la liste noire des adresses associées à des
activités malveillantes en ligne fournies par l’API de AbuseIPDB.<br>
➔ Ajoute des règles de blocage au pare-feu (IPTables).<br>
➔ Envoie d’un rapport quotidien à l’administrateur réseau (par mail).<br>

<h1 align="center"> Script:</h1>

```
#!/bin/bash

#partie1: extration des adresse ip 

cat projet.log | cut -f1 -d' ' > file.txt
key="d94e1d7b0f5e566f047fc3ccb9bba6d99a449f252027c676a01c557797ae33292376ab97edd7b699";

# partie2: la Verification des adresses ip 
input="file.txt"
while IFS= read -r line 
do 
echo "";
curl -G https://api.abuseipdb.com/api/v2/check \--data-urlencode "ipAddress=$line" \-d maxAgeInDays=90 \-d verbose \-H "Key: $key" \-H "Accept: application/json"
done <"$input"

# partie3: Bloquage des adresses ip 
while 
IFS= read -r line 
do 
 echo "";
     sudo iptables -A INPUT -s $line  -j DROP;
     echo "adresse bloquer"
done < "$input"
```


