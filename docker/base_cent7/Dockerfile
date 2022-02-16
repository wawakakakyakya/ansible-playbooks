FROM centos:7 as base_cent7

RUN set -ex \
	: "install packages" \
	&& rm -f /etc/rpm/macros.image-language-conf \
	&& sed -i '/^override_install_langs=/d' /etc/yum.conf \
	&& yum update -y glibc-common \
	&& yum -y reinstall glibc-common \
	&& yum install -y net-tools tcpdump epel-release which sudo wget make \
	&& yum clean all \
	&& echo $'\n\
	export LANG="ja_JP.UTF-8"\n\
	export LANGUAGE="ja_JP:ja"\n\
	export LC_ALL="ja_JP.UTF-8"\n\
	' >> /etc/profile \
	&& source /etc/profile \
	&& localedef -f SHIFT_JIS -i ja_JP /usr/lib/locale/ja_JP.sjis \
	&& echo 'ZONE="Asia/Tokyo"' > /etc/sysconfig/clock \
	&& rm -f /etc/localtime \
	&& ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

ENV LANG="ja_JP.UTF-8"
ENV LANGUAGE="ja_JP:ja"
ENV LC_ALL="ja_JP.UTF-8"
