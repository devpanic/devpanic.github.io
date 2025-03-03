---
title: macOS 로컬에서 Spark 환경 구축하기
date: 2025-03-03 00:00:00 +0800
categories: [Data Engineering, Spark]
tags: [Spark]
---

## 공통
현재 3.5.5 버전이 릴리즈되어 있어서 해당 버전으로 세팅한다. <br>
이 경우 jdk17 버전이 필요하다.
```shell
$ brew install openjdk@17
```

## 1. Spark 설치

**설치**
```shell
$ brew install apache-spark
```
Java JDK 17버전도 함께 설치된다.

**확인**
```shell
$ spark-shell
```

## 2. Pyspark 설치

**설치**
pip 설치가 되어있지 않은 경우 파이썬 설치를 먼저 진행 <br>
pip은 파이썬과 함께 자동으로 설치된다. <br>
25년 3월 3일 기준으로 3.13 버전의 파이썬이 설치된다.

```shell
$ brew install python
```

**pip 설치 확인**
```shell
$ pip3 --version
```

**pyspark 설치 - 1. 가상환경 구축** <br>
바로 설치하려 하면 아래와 같은 에러가 발생한다. <br>
```shell
➜  ~ pip3 install pyspark
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try brew install
    xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a Python library that isn't in Homebrew,
    use a virtual environment:
    
    python3 -m venv path/to/venv
    source path/to/venv/bin/activate
    python3 -m pip install xyz
    
    If you wish to install a Python application that isn't in Homebrew,
    it may be easiest to use 'pipx install xyz', which will manage a
    virtual environment for you. You can install pipx with
    
    brew install pipx
    
    You may restore the old behavior of pip by passing
    the '--break-system-packages' flag to pip, or by adding
    'break-system-packages = true' to your pip.conf file. The latter
    will permanently disable this error.
    
    If you disable this error, we STRONGLY recommend that you additionally
    pass the '--user' flag to pip, or set 'user = true' in your pip.conf
    file. Failure to do this can result in a broken Homebrew installation.
    
    Read more about this behavior here: <https://peps.python.org/pep-0668/>

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```shell
가상 환경을 만들어서 python 라이브러리들을 관리하라는 에러다. <br>
파이썬 가상 환경용 디렉토리를 생성하여 환경 구성을 진행한다.
```
$ mkdir PyEnv    
$ cd PyEnv 
$ python3 -m venv ~/PyEnv/venv
$ source ~/PyEnv/venv/bin/activate
```

**pyspark 설치 - 2. 설치**
```shell
$ (venv) ➜  PyEnv pip3 install jupyter jupyterlab
```

**pyspark 설치 - 3. 설치 확인**
```shell
(venv) ➜  PyEnv pyspark
/Users/devpanic/PyEnv/venv/lib/python3.13/site-packages/pyspark/bin/spark-class: line 71: /opt/homebrew/opt/openjdk@17/bin/java: No such file or directory
/Users/devpanic/PyEnv/venv/lib/python3.13/site-packages/pyspark/bin/spark-class: line 97: CMD: bad array subscript
head: illegal line count -- -1
(venv) ➜  PyEnv pyspark
Python 3.13.2 (main, Feb  4 2025, 14:51:09) [Clang 16.0.0 (clang-1600.0.26.6)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
25/03/03 16:41:26 WARN Utils: Your hostname, Mi-Yeonui-MacBookPro.local resolves to a loopback address: 127.0.0.1; using 192.168.0.103 instead (on interface en0)
25/03/03 16:41:26 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
25/03/03 16:41:26 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 3.5.5
      /_/

Using Python version 3.13.2 (main, Feb  4 2025 14:51:09)
Spark context Web UI available at http://192.168.0.103:4040
Spark context available as 'sc' (master = local[*], app id = local-1740987687086).
SparkSession available as 'spark'.

```