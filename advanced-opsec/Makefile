# GENERIC MAKEFILE
project = advanced-opsec
web_dir = ~/public_html/presentations
web_server = bawls.ehouse.io
theme = night
transition = linear
pandoc = ~/.cabal/bin/pandoc
cabal = $(shell which cabal)

all: $(project).html

$(project).html: $(project).md $(pandoc) Makefile
	$(pandoc) -t revealjs -V theme=$(theme) -V transition=$(transition) -s $(project).md -o $(project).html

$(pandoc): $(cabal)
	cabal install pandoc

reveal.js:
	git clone https://github.com/hakimel/reveal.js.git

deploy: $(project).html reveal.js
	mkdir -p $(web_dir)/$(project)/
	rsync -av reveal.js $(web_server):$(web_dir)/$(project)/
	scp $(project).html $(web_server):$(web_dir)/$(project)/$(project).html

clean:
	rm -rf $(wildcard *.html)

.PHONY: all deploy clean
