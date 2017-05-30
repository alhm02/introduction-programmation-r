### -*-Makefile-*- pour mettre à jour la page web de
### Introduction à la programmation en R
##
## Copyright (C) 2017 Vincent Goulet
##
## Auteur: Vincent Goulet
##
## Ce fichier fait partie du projet Introduction à la programmation en R
## http://github.com/vigou3/introduction-programmation-r


## Variables définies normalement dans ../Makefile, mais répétées ici
## pour pouvoir appeler 'make' seulement dans ce répertoire.
MASTER = ../introduction-programmation-r.tex
VERSION = $(shell grep "newcommand{\\\\ednum" ${MASTER} \
	| cut -d } -f 2 | tr -d {)
VERSIONSTR = $(shell grep "newcommand{\\\\edstr" ${MASTER} \
	| cut -d } -f 2 | tr -d {)
ISBN = $(shell grep "newcommand{\\\\ISBN" ${MASTER} \
	| cut -d } -f 2 | tr -d {)

update-pages:
	awk 'BEGIN { FS = "/"; OFS = "/" } \
	     /^## Édition/ { print; getline; print; getline; \
	                     print "${VERSIONSTR}, ISBN ${ISBN}"; next } \
	     /^\[nouveautés\]/ { $$4 = "edition-${VERSION}" } 1' \
	    index.md > tmpfile && \
	  mv tmpfile index.md
	cd _layouts && \
	  awk 'BEGIN { FS = "/"; OFS = "/" } \
	       /{{ site.github.releases_url }}/ { $$3 = "edition-${VERSION}" } 1' \
	       default.html > tmpfile && \
	  mv tmpfile default.html
	git commit index.md _layouts/default.html \
	    -m "Mise à jour de la page web pour l'édition ${VERSION}" && \
	  git push
