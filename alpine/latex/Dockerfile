ARG base_tag="2.9.2"
FROM pandoc/core:${base_tag}

# NOTE: `libsrvg`, pandoc uses `rsvg-convert` for working with svg images.
# NOTE: to maintainers, please keep this listing alphabetical.
RUN apk --no-cache add \
        freetype \
        fontconfig \
        gnupg \
        gzip \
        librsvg \
        perl \
        tar \
        wget \
        xz \
	make \
	bash \
	python3 \
        python3-dev
	
RUN pip3  install --no-cache-dir --upgrade pip \
  && pip3 install --no-cache-dir sphinx sphinx_rtd_theme \
  && rm -rf /var/cache/* \
  && rm -rf /root/.cache/*	
  

RUN cd /usr/bin \
  # && ln -sf easy_install-3.5 easy_install \
  && ln -sf python3 python \
  && ln -sf pip3 pip
  

# DANGER: this will vary for different distributions, particularly the
# `linuxmusl` suffix.  Alpine linux is a musl libc based distribution, for other
# "more common" distributions, you likely want just `-linux` suffix rather than
# `-linuxmusl` -----------------> vvvvvvvvvvvvvvvv
ENV PATH="/opt/texlive/texdir/bin/x86_64-linuxmusl:${PATH}"
WORKDIR /root

COPY common/latex/texlive.profile /root/texlive.profile
COPY common/latex/install-texlive.sh /root/install-texlive.sh
RUN /root/install-texlive.sh

COPY common/latex/install-tex-packages.sh /root/install-tex-packages.sh
RUN /root/install-tex-packages.sh

RUN rm -f /root/texlive.profile \
          /root/install-texlive.sh \
          /root/install-tex-packages.sh

WORKDIR /data
