FROM \
    ubuntu:18.04

COPY \
    [ "openssl-1.1.1s.tar.gz", "Python-3.7.4.tgz", "get-pip.py", "/workspace/" ]
# curl -L -C - -O https://www.openssl.org/source/openssl-1.1.1s.tar.gz
# curl -L -C - -O https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
# curl -L -C - -O https://github.com/pypa/get-pip/raw/6d265be7a6b5bc4e9c5c07646aee0bf0394be03d/public/get-pip.py

RUN set -eu; \
    cd workspace; \
    apt-get update; \
    DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y --no-install-recommends \
                       make gcc curl perl build-essential \ 
                       checkinstall zlib1g-dev libbz2-dev \
                       liblzma-dev sqlite3 libsqlite3-dev \ 
                       openssl tcl-dev tk-dev dirmngr libncurses5-dev \ 
                       libgdbm-dev libnss3-dev libssl-dev \ 
                       libreadline-dev libffi-dev libc6-dev \ 
                       unzip uuid-dev libbluetooth-dev libgdbm-compat-dev \
                       xz-utils libncursesw5-dev libexpat1-dev; \
    tar -xzf openssl-1.1.1s.tar.gz; \
    cd openssl-1.1.1s; \
    ./config --prefix=/usr/local; \
    make -j `nproc`; \
    make install; \
    make clean; \
    echo "/usr/local/lib">/etc/ld.so.conf.d/openssl-1.1.1s.conf; \
    ldconfig; \
    cd /workspace; \
    tar -xzf Python-3.7.4.tgz; \
    cd /workspace/Python-3.7.4; \
    ./configure \
        --prefix=/usr/local \
		--enable-loadable-sqlite-extensions \
		--enable-option-checking=fatal \
		--enable-shared \
		--with-lto \
                --with-openssl="/usr/local" \
		--with-system-expat \
		--without-ensurepip; \
    make -j `nproc`; \
    make install; \
    make clean; \
    echo "/usr/local/lib">/etc/ld.so.conf.d/python-3.7.4.conf; \
    cd /workspace; \
    ldconfig -v; \
    /usr/local/bin/python3 get-pip.py \
		--disable-pip-version-check \
		--no-cache-dir \
		--no-compile \
		"pip==22.3.1" \
		"setuptools==65.5.1"; \
    /usr/local/bin/pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U; \
    rm -rf /var/lib/apt/lists/*

FROM \
    ubuntu:18.04

LABEL AUTHOR="IVAN DU" E-MAIL="mrivandu@hotmail.com" VERSION="1.0.0" PYTHON-VERSION="3.7.4" BUILD-DATE="20221108"

ENV LANG="C.UTF-8" \
    LC_ALL="C.UTF-8" \
    TZ="Asia/Shanghai"

COPY \
    --from=0 [ "/usr/local", "/usr/local" ]

RUN set -eu; \
    echo "/usr/local/lib">/etc/ld.so.conf.d/python-3.7.4.conf; \
    ldconfig; \
    apt-get update; \
    DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y libssl-dev curl perl vim; \
    DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y zlib1g-dev libbz2-dev libexpat1-dev; \
    DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y liblzma-dev sqlite3 libsqlite3-dev; \
    DEBIAN_FRONTEND="noninteractive" \ 
    apt-get install -y openssl tcl-dev tk-dev libncurses5-dev; \
    DEBIAN_FRONTEND="noninteractive" \ 
    apt-get install -y libgdbm-dev libgdbm-compat-dev libnss3-dev libssl-dev; \
    DEBIAN_FRONTEND="noninteractive" \ 
    apt-get install -y libreadline-dev libffi-dev libc6-dev; \
    DEBIAN_FRONTEND="noninteractive" \ 
    apt-get install -y unzip uuid-dev libbluetooth-dev; \
    DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y xz-utils libncursesw5-dev; \
    ln -s /usr/local/bin/python3 /usr/bin/python; \
    ln -s /usr/local/bin/pip /usr/bin/pip; \
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*; \
    python --version; \
    pip --version

CMD [ "/usr/bin/python" ]
