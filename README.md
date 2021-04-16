Satellite 6 performance tuning guide
====================================

Satellite 6 performance tuning guide

Contribute
==========

Setup your environment:

    sudo dnf -y install python-sphinx-latex latexmk xpdf # to install all dependencies
    python -m venv venv
    source venv/bin/activate
    python -m pip install -r requirements.txt

After changes, check spelling (use `spelling_wordlist.txt` to whitelist):

    make -C docs/ spelling

Build PDF:

    make -C docs/ latexpdf

View PDF:

    xpdf docs/_build/latex/satellite6performancetuningguide.pdf

Cleanup workdir:

    make -C docs/ clean
