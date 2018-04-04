CircleCI Python test repository
===============================
Having experienced caching issues on CircleCI, I'm using this repository to
investigate the behaviour of CircleCI with various Python package/repo setups.

Conclusions
-----------
* Having `-e .` in `requirements.txt` will cause
  `pip install -r requirements.txt` to fail on CircleCI
  (it works only when doing _Rebuild without cache_, normal builds will fail)
* Without `requirements.txt`, CircleCI will run `python setup.py install` which
  does work (not tested under caching conditions/multiple subsequent builds)
