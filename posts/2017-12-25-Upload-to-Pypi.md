title: "Distribute to Pypi"
date: 2017-12-25 15:56:00 +0800
Category: Python
Tags: Pypi, Package, Distribution
Has_Math: True

##準備動作##

到 [Pypi](https://pypi.python.org/pypi) 註冊 username 以及 password

並安裝需要的 packages:

	pip install -U pip setuptools twine

##製作 ~/.pypirc##
	
	[distutils]
		index-servers = pypi

	[pypi]
		username: (向 Pypi 註冊的 account)
		password:
		
##上傳##

	python setup.py sdist
	twine upload dist/*


本篇文章原本是要解決 Failed to upload packages to PyPI: 410 Gone 的問題,
來源: [https://stackoverflow.com/questions/45207128/failed-to-upload-packages-to-pypi-410-gone](https://stackoverflow.com/questions/45207128/failed-to-upload-packages-to-pypi-410-gone)



