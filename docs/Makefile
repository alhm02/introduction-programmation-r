### -*-Makefile-*- pour mettre à jour la page web de
### Introduction à la programmation en R
##
## Copyright (C) 2017 Vincent Goulet
##
## Auteur: Vincent Goulet
##
## Ce fichier fait partie du projet Introduction à la programmation en R
## http://github.com/vigou3/introduction-programmation-r


## Variables définies dans ../Makefile répétées ici pour appels
## directs dans le présent répertoire.
MASTER = ../introduction-programmation-r.tex
VERSION = $(shell grep "newcommand{\\\\ednum" ${MASTER} \
	| cut -d } -f 2 | tr -d {)
VERSIONSTR = $(shell grep "newcommand{\\\\edstr" ${MASTER} \
	| cut -d } -f 2 | tr -d {)
ISBN = $(shell grep "newcommand{\\\\ISBN" ${MASTER} \
	| cut -d } -f 2 | tr -d {)

## Fichiers à modifier.
INDEX = index.md
DEFAULT = default.html

all: files commit

files:
	awk 'BEGIN { FS = "/"; OFS = "/" } \
	     /^## Édition/ { print; getline; print; getline; \
	                     print "${VERSIONSTR}, ISBN ${ISBN}"; next } \
	     /^\[nouveautés\]/ { $$4 = "edition-${VERSION}" } 1' \
	    ${INDEX} > tmpfile && \
	  mv tmpfile ${INDEX}
	cd _layouts && \
	  awk 'BEGIN { FS = "/"; OFS = "/" } \
	       /{{ site.github.releases_url }}/ { $$3 = "edition-${VERSION}" } 1' \
	       ${DEFAULT} > tmpfile && \
	  mv tmpfile ${DEFAULT}

commit:
	git commit ${INDEX} _layouts/${DEFAULT} \
	    -m "Mise à jour de la page web pour l'édition ${VERSION}" && \
	  git push
