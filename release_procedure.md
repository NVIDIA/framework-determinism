# Release Procedure

This is the procedure to follow to finalize a release of
`tensorflow-determinism`. See the official python documentation on
[setuptools][1].

## 1. Merge Release Branch

Thoroughly review the release branch (e.g. `r0.3`) and merge it into master.
Then delete the release branch.

## 2. Check Version Number

Confirm that the version number in `fwd9m/version.py` is correct.

## 3. Run Tests

Run all the tests and make sure they all pass.

```
$cd test
$./all.sh
```

## 4. Create a Source Distribution

```
python3 setup.py sdist
```

Note that to install the source distribution, the user will need to have
pip installed a new enough version of `setuptools` and also `wheel`.

## 5. Create a Universal Weel

```
python3 setup.py bdist_wheel
```

Note that `config.cfg` specifies that wheels are universal by default.

## 6. Upload to PyPI

Upload the source distribution and the universal wheel to the Python Package
Index (PyPI).

The following assumes that `$HOME/.pypirc` exists and contains the following:

```
distutils]
index-servers =
    pypi
    testpypi

[pypi]
# Use upload tool's default repository URL
username: <username>

[testpypi]
repository: https://test.pypi.org/legacy/
username: <username>
```

### 5a. Test PyPI Server


```
twine upload --repository testpypi dist/framework-determinism-<version>*
twine upload --repository testpypi dist/framework_determinism-<version>*
```

Review the release online and try installing it.

```
cd ~/temp
python3 -m venv venv
venv/bin/pip install -i https://test.pypi.org/simple/ framework-determinism
```

### 5b. Real PyPI Server

```
twine upload --repository pypi dist/framework-determinism-<version>*
twine upload --repository pypi dist/framework_determinism-<version>*
```

Again, review the release online and try installing it.

```
cd ~/temp
python3 -m venv venv
venv/bin/pip install framework-determinism
```

## 6. Create GitHub Release

Finally, on GitHub, create a new release with an appropriate version tag
(e.g. `v0.1.0`).

[1]: https://packaging.python.org/guides/distributing-packages-using-setuptools/
