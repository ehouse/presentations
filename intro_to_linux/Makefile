project = intro_to_linux
web_dir = ~/public_html/presentations
theme = night
transition = slide
pandoc = ~/.cabal/bin/pandoc
cabal = $(shell which cabal)

all: $(project).html

$(project).html: $(project).md $(pandoc)
	$(pandoc) -t revealjs -V theme=$(theme) -V transition=$(transition) -s $(project).md -o $(project).html

$(pandoc): $(cabal)
	cabal install pandoc

reveal.js:
	git clone https://github.com/hakimel/reveal.js.git

install: $(project).html reveal.js
	mkdir -p $(web_dir)/$(project)/
	rsync -av reveal.js $(web_dir)/$(project)/
	rsync -av $(project).html $(web_dir)/$(project)/

clean: 
	rm -rf reveal.js
	rm -rf $(project).html

.PHONY: all install-deps install clean
