CircleCI Python test repository
===============================
Having experienced caching issues on CircleCI, I'm using this repository to 
investigate the behaviour of CircleCI with various Python package/repo setups.

Conclusions
-----------
* Having `-e .` in `requirements.txt` without a versioned package will cause
  `pip install -r requirements.txt` to fail on CircleCI.
* Without `requirements.txt`, CircleCI will run `python setup.py install` which
  does work (not tested under caching conditions/multiple subsequent builds)
