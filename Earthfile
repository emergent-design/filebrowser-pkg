VERSION 0.8

noble:
 	FROM ubuntu:24.04

core:
	ARG DISTRIBUTION=noble
	FROM +$DISTRIBUTION
	ENV DEBIAN_FRONTEND noninteractive
	ENV DEBCONF_NONINTERACTIVE_SEEN true
	WORKDIR /code

	RUN apt-get -q update && apt-get install -y --no-install-recommends \
		ca-certificates build-essential curl fakeroot chrpath gnupg2 dh-exec devscripts libdistro-info-perl

package:
	ARG TARGETARCH
	ARG FILEBROWSER_VERSION=1.5.0-beta
	ARG DISTRIBUTION=noble
	FROM +core

	COPY --dir package .

	RUN curl -so package/filebrowser-quantum -L "https://github.com/gtsteffaniak/filebrowser/releases/download/v${FILEBROWSER_VERSION}/linux-${TARGETARCH}-filebrowser"
	RUN chmod +x package/filebrowser-quantum

	ARG DOWNSTREAM=1
	RUN cd package \
		&& dch --distribution=unstable -Mv "${FILEBROWSER_VERSION}-${DOWNSTREAM}" "New upstream release" \
		&& dpkg-buildpackage -b -uc -us -v"${FILEBROWSER_VERSION}-${DOWNSTREAM}"

	SAVE ARTIFACT --keep-ts *.deb AS LOCAL build/$DISTRIBUTION/
