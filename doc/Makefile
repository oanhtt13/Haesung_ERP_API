# Minimal makefile for Sphinx documentation
#

# You can set these variables from the command line, and also
# from the environment for the first two.
SPHINXOPTS    ?=
SPHINXBUILD   ?= sphinx-build
SOURCEDIR_VI     = ./vi
BUILDDIR_VI      = _build/vi
SOURCEDIR_EN     = ./jp
BUILDDIR_EN      = _build/jp

# Put it first so that "make" without argument is like "make help".
help:
	@$(SPHINXBUILD) -M help "$(SOURCEDIR_VI)" "$(BUILDDIR_VI)" $(SPHINXOPTS) $(O)
	@$(SPHINXBUILD) -M help "$(SOURCEDIR_EN)" "$(BUILDDIR_EN)" $(SPHINXOPTS) $(O)

.PHONY: help Makefile

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
%: Makefile
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR_VI)" "$(BUILDDIR_VI)" $(SPHINXOPTS) $(O)
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR_EN)" "$(BUILDDIR_EN)" $(SPHINXOPTS) $(O)