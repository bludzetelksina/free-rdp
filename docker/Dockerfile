FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Устанавливаем git и Python (для websockify)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        python3 \
        python3-websockify \
        novnc && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Клонируем noVNC и websockify
RUN git clone https://github.com/novnc/noVNC.git /opt/novnc && \
    git clone https://github.com/novnc/websockify /opt/novnc/utils/websockify && \
    ln -s /opt/novnc/vnc_lite.html /opt/novnc/index.html

# Устанавливаем timezone и нужные пакеты в одном RUN, отключая интерактивность
RUN apt-get update && \
    apt-get install -y --no-install-recommends tzdata && \
    ln -fs /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata && \
    apt-get install -y --no-install-recommends \
        xrdp \
        xorgxrdp \
        xfce4 \
        xfce4-terminal \
        tightvncserver \
        xfonts-base \
        firefox \
        wget \
        curl \
        ca-certificates \
        supervisor && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN echo "startxfce4" > /etc/skel/.xsession && \
    echo "startxfce4" > /root/.xsession

COPY config/xrdp.ini /etc/xrdp/xrdp.ini
COPY config/sesman.ini /etc/xrdp/sesman.ini

COPY docker/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

EXPOSE 3389 5901 6080

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
