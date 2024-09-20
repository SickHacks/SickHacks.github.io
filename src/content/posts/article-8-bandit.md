---
title: Bandit - OverTheWire
published: 2024-09-20
description: 'Write Up de los ejercicios de Bandit - OverTheWire'
image: ''
tags: [writeups]
category: 'WriteUps'
draft: false 
lang: 'es'
---

## Bandit 0
"The flag is stored in a file called readme located in the home directory."
```
ls -> README -> cat README
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```
_______________
## Bandit 1
"The flag is stored in a file called - located in the home directory"
```
ls -> - -> cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```
_______________
## Bandit 2
"The flag is stored in a file called spaces in this filename located in the home directory"
```
ls -> spaces in this filename -> cat "spaces in this filename"
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```
_______________
## Bandit 3
"The flag is stored in a hidden file in the inhere directory"
```	
ls -> inhere -> cd inhere -> (hidden files) -> ls -a -> .  ..  .hidden -> cat .hidden
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe		
```
_______________
## Bandit 4
"The flag is stored in the only human-readable file in the inhere directory"
```	
ls -> inhere -> cd inhere -> -file00 -file01 -file02 -file03 -file04 -file05 -file06 -file07 -file08 -file09 -> cd (salimos de la carpeta inhere)
   -> file inhere/* (muestrame todos los tipos de archivos de la carpeta "inhere") -> inhere/-file00: OpenPGP Public Key
								        		    
                                                    inhere/-file01: data
										            inhere/-file05: data
										            inhere/-file06: data
										            inhere/-file07: ASCII text
                                                    inhere/-file08: data

-> cd inhere -> cat ./-file07  -->  lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```
_______________
## Bandit 5
"The flag is stored in a file somewhere under the inhere directory and has all of the following properties:"
1. Human-readable	
2. 1033 bytes in size	
3. Not executable
```
ls -> inhere --> find . -type f -readable -size 1033c ! -executable --> ./inhere/maybehere07/.file2 --> cat ./inhere/maybehere07/.file2 | xargs
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU 
```
_______________
## Bandit 6
"The flag is stored somewhere on the server and has all of the following properties:"
1. Owned by user bandit7	
2. Owned by group bandit6	
3. 33 bytes in size
(Cuando entras y usas ls no hay nada, asi que se debe buscar desde la raiz, osea con /)
```
ls --> (empty) --> find / -user bandit7 -group bandit6 -size 33c 
```
(En este momento sale una cantidad exagerada de archivos pero con Permission denied, 
por lo tanto tenemos que usar 2>/dev/null, el cual nos permite redireccionar todos los errores a Null el cual hace funcion de agujero negro)
Quedaria de esta forma:
```
--> find / -user bandit7 -group bandit6 -size 33c 2>/dev/null --> /var/lib/dpkg/info/bandit7.password
```	
(Para ver la flag podemos usar 2 formas)
```
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```
ó
```
cat /var/lib/dpkg/info/bandit7.password
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S  
```
_______________
#### Bandit7
	The flag is stored in the file data.txt next to the word millionth
	
	ls --> data.txt --> cat data.txt | wc -l --> (wc -l significa words count -l lines) 98567 --> grep "millionth" data.txt
	--> millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP
	Ahora bien, puedes jugar en el tema tipo: grep "^m" (Buscara todas las lineas que partan con m, para buscar algo en lo que termine se usa $) 
						  EJ: grep "^hola$" data.txt (busca todas las palabras que inicien con H y terminen con a)
						  grep "millionth" data.txt -n (muestra en que linea se ubica) --> 3521:millionth TESKZC0XvTetK0S9xNwm25STk5iWrBvP
						  awk 'NR==3521' data.txt (muestra que hay en esa linea) --> millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP  

	Igualmente: millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP, no esta del todo correcto al verlo con: $ cat data.txt | grep "millionth", debido a que te muestra la linea completa y en realidad nosotros queremos ver solo el segundo argunmento, el cual es la flag, para esto podemos usar: awk
	$ cat data.txt | grep "millionth" | awk '{print $2}'


#### Bandit8
	The flag is stored in the file data.txt and is the only line of text that occurs only once

	Si utilizas "sort" el contenido del archivo se ordena alfabeticamente
	Con "uniq -u" puedes listar lineas unicas
	ls --> data.txt --> cat data.txt | sort | uniq -u
	Tambien puedes hacerlo con:    $ sort data.txt | uniq -u
	EN632PlfYiZbn3PhVK3XOGSlNInNE00t


#### Bandit9
	The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

	Cuando un archivo no es legible puedes utilizar el comando "strings" que es para listar las cadenas de caracteres imprimibles de un archivo 
	ls --> data.txt --> strings data.txt | grep "===="
	========== the#
	========== password
	========== is
	========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

	G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s


#### Bandit10
	The password for the next level is stored in the file data.txt, which contains base64 encoded data

	"base64" : Base64 es un sistema de numeración posicional que usa 64 como base. Es la mayor potencia que puede ser representada usando únicamente los caracteres imprimibles de ASCII. Esto ha propiciado su uso para codificación de correos electrónicos, PGP y otras aplicaciones. 

	Puedes ver el manual de base64 utilizando el comando: $ man base64, aqui indica que puedes decodificar un archivo usando el paramentro -d

	ls --> data.txt --> cat data.txt --> VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjJSS05kTllGTmI2blZDS3pwaGxYSEJNCg==
	El contenido del archivo esta codificado
	$ base64 -d data.txt --> The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

	6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

	"Como informacion extra, puedes crear una cadena en base64 de la siguiente manera:"
	$ echo "Hola esto es una prueba" | base64 --> SG9sYSBlc3RvIGVzIHVuYSBwcnVlYmEK
	$ echo "SG9sYSBlc3RvIGVzIHVuYSBwcnVlYmEK" | base64 -d --> Hola esto es una prueba


