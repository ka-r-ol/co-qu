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
	apk del build-deps && rm -rf /var/cache/apk/ && \
	apk add --no-cache --update vim 

#	chown -R co-qu:co-qu /home/co-qu/iks 

EXPOSE  4200

USER 	co-qu
#RUN	git clone https://github.com/ka-r-ol/iks.git  /home/co-qu/iks 

CMD ["/shellinaboxd", "--no-beep", "--disable-peer-check", "--disable-ssl", "--service=/:co-qu:co-qu:/:/bin/bash /work/.submodules/start_menu.sh"]
