# GENERIC MAKEFILE
web_dir     = ~/public_html/presentations
web_server  = bawls.ehouse.io
MD_BLD     := $(patsubst %.md, %.pdf, $(wildcard *.md))

all: $(MD_BLD)

%.pdf: %.md Makefile
	pandoc -t beamer -V theme:metropolis $< -o $@

deploy: $(MD_BLD) Makefile
	rsync -av $< $(web_server):$(web_dir)/

clean:
	rm -rf $(wildcard *.pdf)

.PHONY: all deploy clean
