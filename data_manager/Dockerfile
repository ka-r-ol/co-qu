FROM python:3.10-alpine

COPY image_data .

RUN     apk update && apk upgrade && apk add bash && \
	apk add --no-cache --update --virtual build-deps alpine-sdk autoconf automake libtool curl tar git && \
	\
	adduser -D co-qu -s /bin/bash && \
	\
	git clone https://github.com/shellinabox/shellinabox.git /shellinabox && \
	cd /shellinabox && \
	git apply /alpine.patch && \
	rm /alpine.patch && \
	autoreconf -i && \
	./configure --prefix=/shellinabox/bin && \
	make && make install && cd / && \
	\
	mv /shellinabox/bin/bin/shellinaboxd /shellinaboxd && \
	rm -rf /shellinabox && \
	\
	apk del build-deps && rm -rf /var/cache/apk/ 

RUN	apk add --no-cache --update vim sqlite

RUN apk add --no-cache git \
	zip unzip \
	ca-certificates \
	curl \
	openjdk11-jre 
#openjdk8-jre 
ENV SONAR_SCANNER_VERSION 4.0.0.1744
#ENV SONAR_SCANNER_VERSION 4.6.2.2472 
#ENV SONAR_SCANNER_VERSION 4.5.0.2216
ADD https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}.zip /opt/sonar-scanner.zip
RUN unzip /opt/sonar-scanner.zip -d /opt && \
	rm /opt/sonar-scanner.zip && \
	ln -s /opt/sonar-scanner-${SONAR_SCANNER_VERSION}/bin/sonar-scanner /usr/bin/sonar-scanner

############################33
# ARG SONAR_SCANNER_HOME=/opt/sonar-scanner
# ENV SONAR_SCANNER_VERSION 4.5.0.2216
# ARG UID=1000
# ARG GID=1000
# ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk \
# 	HOME=/tmp \
# 	XDG_CONFIG_HOME=/tmp \
# 	SONAR_SCANNER_HOME=${SONAR_SCANNER_HOME} \
# 	SONAR_USER_HOME=${SONAR_SCANNER_HOME}/.sonar \
# 	PATH=/opt/java/openjdk/bin:${SONAR_SCANNER_HOME}/bin:${PATH} \
# 	NODE_PATH=/usr/lib/node_modules \
# 	SRC_PATH=/usr/src \
# 	LANG=en_US.UTF-8 \
# 	LC_ALL=en_US.UTF-8

# WORKDIR /opt

# RUN set -eux; \
# 	addgroup -S -g ${GID} scanner-cli; \
# 	adduser -S -D -u ${UID} -G scanner-cli scanner-cli; \
# 	apk add --no-cache --virtual build-dependencies wget unzip gnupg; \
# 	apk add --no-cache git python3 py-pip bash shellcheck 'nodejs>12' 'npm>7' openjdk11-jre curl musl-locales musl-locales-lang; \
# 	wget -U "scannercli" -q -O /opt/sonar-scanner-cli.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}.zip; \
# 	wget -U "scannercli" -q -O /opt/sonar-scanner-cli.zip.asc https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}.zip.asc; \
# 	for server in $(shuf -e ha.pool.sks-keyservers.net \
# 	hkp://p80.pool.sks-keyservers.net:80 \
# 	keyserver.ubuntu.com \
# 	hkp://keyserver.ubuntu.com:80 \
# 	pgp.mit.edu) ; do \
# 	gpg --batch --keyserver "${server}" --recv-keys 679F1EE92B19609DE816FDE81DB198F93525EC1A && break || : ; \
# 	done; \
# 	gpg --verify /opt/sonar-scanner-cli.zip.asc /opt/sonar-scanner-cli.zip; \
# 	unzip sonar-scanner-cli.zip; \
# 	rm sonar-scanner-cli.zip sonar-scanner-cli.zip.asc; \
# 	mv sonar-scanner-${SONAR_SCANNER_VERSION} ${SONAR_SCANNER_HOME}; \
# 	pip install --no-cache-dir --upgrade pip; \
# 	pip install --no-cache-dir pylint; \
# 	apk del --purge build-dependencies; \
# 	mkdir -p "${SRC_PATH}" "${SONAR_USER_HOME}" "${SONAR_USER_HOME}/cache"; \
# 	chown -R scanner-cli:scanner-cli "${SONAR_SCANNER_HOME}" "${SRC_PATH}"; \
# 	chmod -R 777 "${SRC_PATH}" "${SONAR_USER_HOME}";


##############################

EXPOSE  4200

USER 	co-qu

CMD ["/shellinaboxd", "--no-beep", "--disable-peer-check", "--disable-ssl", "--service=/:co-qu:co-qu:/:/bin/bash /work/.submodules/start_menu.sh"]
