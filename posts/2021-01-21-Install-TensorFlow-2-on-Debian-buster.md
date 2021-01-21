title: Install TensorFlow-2 on Debian buster
slug: 2021-01-21-install-tensorflow-2-on-debian-buster
date: 2021-01-21 11:34:55 UTC+08:00
type: text
Category: Machine Learning


Debian buster 提供的 pip version 只到 18.1，所以透過 pip 安裝無法滿足
TensorFlow 2.0 需求(pip >= 19.0)

所以可以用 venv 在裡面將 pip 升級到 20.0

首先安裝 buster 提供的套件：

	sudo apt update
	sudo apt install python3-dev python3-pip python3-venv

然後安裝 venv, 先跳到工作目錄：

	cd [working path]
	python3 -m venv ENV
	source ENV/bin/activate
	; 先更新 wheel, 後續安裝需要
	pip install wheel
	; 再更新 pip
	pip install --upgrade pip

此時 pip 應該已經升級到 20.0 以上的版次, 可以裝 TF 2.0 啦！

	pip install tensorflow

打完收工
