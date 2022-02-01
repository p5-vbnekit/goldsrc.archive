FROM debian:stable

ENV LANG C
ENV LC_ALL C
ENV container docker
ENV DEBIAN_FRONTEND noninteractive

RUN dpkg --add-architecture i386
COPY sources.list /etc/apt/sources.list
RUN apt update --assume-yes && apt install --assume-yes apt-utils
RUN apt update --assume-yes && apt install --assume-yes debconf
RUN apt update --assume-yes && apt full-upgrade --assume-yes
RUN _packages=`apt-mark showmanual` && (test -n "${_packages}" && apt-mark auto ${_packages}) || true

RUN echo locales locales/locales_to_be_generated select en_US.UTF-8 UTF-8 | debconf-set-selections
RUN echo locales locales/default_environment_locale select en_US.UTF-8 | debconf-set-selections
RUN apt update --assume-yes && apt install --assume-yes locales

RUN apt update --assume-yes && apt install --assume-yes python3

RUN apt update --assume-yes && apt install --assume-yes libsdl2-dev:i386
RUN echo steam steam/license note '' | debconf-set-selections
RUN echo steam steam/question select "I AGREE" | debconf-set-selections
RUN apt update --assume-yes && apt install --assume-yes steamcmd

RUN apt update --assume-yes && apt full-upgrade --assume-yes && apt autoremove --purge --assume-yes
RUN _packages=`dpkg -l | grep '^rc' | awk '{print $2}'` && (test -n "${_packages}" && dpkg --purge ${_packages}) || true
RUN apt clean --assume-yes && for _path in /tmp /var/tmp /var/lib/apt/lists; do find "${_path}" -mindepth 1 -maxdepth 1 -exec rm -rf {} \;; done

COPY entry_point.py /usr/local/bin/entry_point
RUN chmod +x /usr/local/bin/entry_point

ENTRYPOINT ["/usr/local/bin/entry_point"]
