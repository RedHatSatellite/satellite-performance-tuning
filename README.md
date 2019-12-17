Satellite 6 performance tuning guide
====================================

Satellite 6 performance tuning guide

Contribute
==========

Setup your environment:

    dnf install python-sphinx-latex   # to install all dependencies
    virtualenv venv
    source venv/bin/activate
    pip install -r requirements.txt

After changes, check spelling (use `spelling_wordlist.txt` to whitelist):

    make -C docs/ spelling

Build PDF:

    make -C docs/ latexpdf
    xpdf docs/_build/latex/satellite6performancetunningguide.pdf

Cleanup workdir:

    make -C docs/ clean
