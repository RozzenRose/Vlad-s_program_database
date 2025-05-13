#docker 

![[Pasted image 20250507172818.png]]
`Dockerfile` - это текстовый файл, содержащий набор инструкций, которые **Docker Engine** использует для сборки **Docker Image**. Он описывает, как создать образ, содержащий все необходимые файлы и настройки для запуска приложения в контейнере. `Dockerfile` - ключевой элемент в работе с **Docker**, обеспечивающий повторяемость и портативность приложений.

**Структура `Dockerfile`**:
`Dockerfile` - это набор инструкций, каждая из которых описывает отдельную операцию по созданию слоя в образе. Инструкции записываются в файле в виде строк. Каждая инструкция имеет определенный синтаксис:
```
[инструкция] [аргументы]
```

**Примеры инструкций**:
- **`FROM`:** Указывает базовый образ, на основе которого будет создаваться новый образ. Например: `FROM ubuntu:latest`. Это первый и обязательный шаг.
- **`LABEL`:** Добавляет метаданные к образу.
- **`MAINTAINER`:** Указывает имя и электронную почту разработчика. (рекомендуется использовать `LABEL` вместо `MAINTAINER` для более гибких метаданных.)
- **`RUN`:** Выполняет команду в контейнере во время сборки образа. Например, устанавливает пакеты (`RUN apt-get update && apt-get install -y nginx`), копирует файлы (`RUN cp /path/to/file /new/destination`) или выполняет скрипты.
- **`COPY` и `ADD`:** Копирует файлы из хост-системы в контейнер.  `COPY` копирует только указанные файлы/каталоги, а `ADD` также может распаковывать архивы.
- **`WORKDIR`:** Устанавливает рабочую директорию в контейнере. Это важно для последующих инструкций `RUN`, `COPY`, `CMD`.
- **`CMD`:** Устанавливает команду, которая будет выполняться при запуске контейнера. Она выполняется только один раз.
- **`ENTRYPOINT`:** Определяет основную точку входа для запуска приложения в контейнере. Команда, указанная в `ENTRYPOINT`, будет выполняться _перед_ командой, указанной в `CMD`. Это позволяет изменять поведение запуска без изменения образа.
- **`ENV`:** Устанавливает переменные среды в контейнере.
- **`EXPOSE`:** Объявляет порты, которые приложение должно слушать. Это необходимо для доступа к приложению извне контейнера.
- **`VOLUME`:** Создает тома (volumes) для сохранения данных, которые не должны теряться при остановке контейнера. Они не входят в сам образ, но могут быть смонтированы во время запуска контейнера.
- **`USER`:** Устанавливает пользователя для выполнения команд.
- **`ARG`:** Объявляет аргументы, которые могут быть переданы при сборке образа.

**Порядок инструкций**:
Инструкции в `Dockerfile` выполняются последовательно. Важно, чтобы порядок инструкций соответствовал логике сборки образа. 
**Пример `Dockerfile`**:
```text
FROM ubuntu:latest

RUN apt-get update && apt-get install -y nginx

COPY ./nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
Этот `Dockerfile` создает образ, содержащий **nginx**, с копией файла конфигурации `nginx.conf` из текущей директории. При запуске контейнера **nginx** будет запущен.

Подробности и нюансы можно почерпнуть из официальной документации.
```text
#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "apply-templates.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#

FROM debian:bookworm-slim

# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# runtime dependencies
RUN set -eux; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		ca-certificates \
		netbase \
		tzdata \
	; \
	rm -rf /var/lib/apt/lists/*

ENV GPG_KEY 7169605F62C751356D054A26A821E680E5FA6305
ENV PYTHON_VERSION 3.12.0

RUN set -eux; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		dpkg-dev \
		gcc \
		gnupg \
		libbluetooth-dev \
		libbz2-dev \
		libc6-dev \
		libdb-dev \
		libexpat1-dev \
		libffi-dev \
		libgdbm-dev \
		liblzma-dev \
		libncursesw5-dev \
		libreadline-dev \
		libsqlite3-dev \
		libssl-dev \
		make \
		tk-dev \
		uuid-dev \
		wget \
		xz-utils \
		zlib1g-dev \
	; \
	\
	wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz"; \
	wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc"; \
	GNUPGHOME="$(mktemp -d)"; export GNUPGHOME; \
	gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$GPG_KEY"; \
	gpg --batch --verify python.tar.xz.asc python.tar.xz; \
	gpgconf --kill all; \
	rm -rf "$GNUPGHOME" python.tar.xz.asc; \
	mkdir -p /usr/src/python; \
	tar --extract --directory /usr/src/python --strip-components=1 --file python.tar.xz; \
	rm python.tar.xz; \
	\
	cd /usr/src/python; \
	gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
	./configure \
		--build="$gnuArch" \
		--enable-loadable-sqlite-extensions \
		--enable-optimizations \
		--enable-option-checking=fatal \
		--enable-shared \
		--with-lto \
		--with-system-expat \
		--without-ensurepip \
	; \
	nproc="$(nproc)"; \
	EXTRA_CFLAGS="$(dpkg-buildflags --get CFLAGS)"; \
	LDFLAGS="$(dpkg-buildflags --get LDFLAGS)"; \
	LDFLAGS="${LDFLAGS:--Wl},--strip-all"; \
	make -j "$nproc" \
		"EXTRA_CFLAGS=${EXTRA_CFLAGS:-}" \
		"LDFLAGS=${LDFLAGS:-}" \
		"PROFILE_TASK=${PROFILE_TASK:-}" \
	; \
# https://github.com/docker-library/python/issues/784
# prevent accidental usage of a system installed libpython of the same version
	rm python; \
	make -j "$nproc" \
		"EXTRA_CFLAGS=${EXTRA_CFLAGS:-}" \
		"LDFLAGS=${LDFLAGS:--Wl},-rpath='\$\$ORIGIN/../lib'" \
		"PROFILE_TASK=${PROFILE_TASK:-}" \
		python \
	; \
	make install; \
	\
	cd /; \
	rm -rf /usr/src/python; \
	\
	find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
			-o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name 'libpython*.a' \) \) \
		\) -exec rm -rf '{}' + \
	; \
	\
	ldconfig; \
	\
	apt-mark auto '.*' > /dev/null; \
	apt-mark manual $savedAptMark; \
	find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec ldd '{}' ';' \
		| awk '/=>/ { so = $(NF-1); if (index(so, "/usr/local/") == 1) { next }; gsub("^/(usr/)?", "", so); printf "*%s\n", so }' \
		| sort -u \
		| xargs -r dpkg-query --search \
		| cut -d: -f1 \
		| sort -u \
		| xargs -r apt-mark manual \
	; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*; \
	\
	python3 --version

# make some useful symlinks that are expected to exist ("/usr/local/bin/python" and friends)
RUN set -eux; \
	for src in idle3 pydoc3 python3 python3-config; do \
		dst="$(echo "$src" | tr -d 3)"; \
		[ -s "/usr/local/bin/$src" ]; \
		[ ! -e "/usr/local/bin/$dst" ]; \
		ln -svT "$src" "/usr/local/bin/$dst"; \
	done

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 23.2.1
# https://github.com/pypa/get-pip
ENV PYTHON_GET_PIP_URL https://github.com/pypa/get-pip/raw/9af82b715db434abb94a0a6f3569f43e72157346/public/get-pip.py
ENV PYTHON_GET_PIP_SHA256 45a2bb8bf2bb5eff16fdd00faef6f29731831c7c59bd9fc2bf1f3bed511ff1fe

RUN set -eux; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get update; \
	apt-get install -y --no-install-recommends wget; \
	\
	wget -O get-pip.py "$PYTHON_GET_PIP_URL"; \
	echo "$PYTHON_GET_PIP_SHA256 *get-pip.py" | sha256sum -c -; \
	\
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark > /dev/null; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*; \
	\
	export PYTHONDONTWRITEBYTECODE=1; \
	\
	python get-pip.py \
		--disable-pip-version-check \
		--no-cache-dir \
		--no-compile \
		"pip==$PYTHON_PIP_VERSION" \
	; \
	rm -f get-pip.py; \
	\
	pip --version

CMD ["python3"]
```

Мы видим, что у него задан базовый образ **Debian Bookworm** (`debian: bookworm-slim`), тег     `-slim` в названии которого обозначает что используется облегченный образ, из которого удалено все лишнее, такое как справочные страницы, документация и прочее, которое не нужно для нормального функционирования контейнера.

Далее идут команды установки необходимых пакетов, таких как инструменты для компиляции, утилиты и различные библиотеки. 

Затем идет скачивание архива исходников **Python** версии `3.12.0`, распаковка, процесс компиляции и установка **Python**.

Далее происходит скачивание скрипта `get_pip.py` и его запуск - происходит установка `pip` версии `23.2.1`