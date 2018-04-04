CircleCI Python test repository
===============================
Having experienced caching issues on CircleCI, I'm using this repository to
investigate the behaviour of CircleCI with various Python package/repo setups.

Partial conclusions
-------------------
* Having `requirements.txt` at the root of the repository will cause CircleCI's
  inference to run `pip install -r requirements.txt`
* Having `setup.py` at the root of the repository will cause CircleCI's
  inference to run `python setup.py install`.
* If both `requirements.txt` and `setup.py` are present both commands are run.
  First `pip install -r requirements.txt`, then `python setup.py install`
* Having `-e .` in `requirements.txt` will cause
  `pip install -r requirements.txt` to fail on CircleCI
  (it works only when doing _Rebuild without cache_, normal builds will fail)
* Without `requirements.txt`, CircleCI will run `python setup.py install` which
  does work (not tested under caching conditions/multiple subsequent builds)

Conclusion
----------
* Installing the package twice is what's causing the issue, once via `-e .`
  in `requirements.txt` and then twice with `python setup.py install`
* Not running `python setup.py install` makes it work
* Use `override` in the `dependencies` section, e.g.

  ```
  dependencies:
    override:
      - pip install -r requirements.txt
  ```

  in order to disable `python setup.py install` when a `setup.py`-file is present
  in your repository