FROM centos:7
MAINTAINER Thabang Maahlo <public@teleforge.co.za>
ENV build_date 2018-03-12

# Update & Install prerequisite
RUN yum update -y && \
    yum install -y epel-release && \
    yum install \
        git \
        kernel-headers \
        gcc \
        gcc-c++ \
        cpp \
        ncurses \
        ncurses-devel \
        libxml2 \
        libxml2-devel \
        sqlite \
        sqlite-devel \
        openssl-devel \
        newt-devel \
        kernel-devel \
        libuuid-devel \
        net-snmp-devel \
        xinetd \
        tar \
        jansson-devel \
        make \
        bzip2 \
        pjproject-devel \
        libsrtp-devel \
        gsm-devel \
        patch \
        speex-devel \
        gettext \
        vim \
        -y

# https://github.com/CentOS/sig-cloud-instance-images/issues/71
RUN localedef -i en_ZA -f UTF-8 en_ZA.UTF-8

# Download asterisk.
WORKDIR /usr/src
RUN git clone -b 15 --depth 1 https://github.com/asterisk/asterisk.git

WORKDIR /usr/src/asterisk

# Configure
RUN ./configure --libdir=/usr/lib64 1> /dev/null
RUN make menuselect.makeopts
RUN menuselect/menuselect \
    --disable BUILD_NATIVE \
    --enable app_stasis \
    --enable chan_bridge_media \
    --enable chan_sip \
    --enable chan_rtp \
    --enable res_http_websocket \
    --enable res_hep_rtcp \
    --enable res_rtp_asterisk \
    --enable res_ari \
    --enable res_ari_applications \
    --enable res_ari_asterisk \
    --enable res_ari_bridges \
    --enable res_ari_channels \
    --enable res_ari_device_states \
    --enable res_ari_endpoints \
    --enable res_ari_events \
    --enable res_ari_mailboxes \
    --enable res_ari_model \
    --enable res_ari_playbacks \
    --enable res_ari_recordings \
    --enable res_ari_sounds \
    --enable res_stasis \
    --enable res_stasis_answer \
    --enable res_stasis_device_state \
    --enable res_stasis_mailbox \
    --enable res_stasis_playback \
    --enable res_stasis_recording \
    --enable res_stasis_snoop \
    --enable res_stasis_test \
    --enable res_statsd \
    --enable res_timing_timerfd \
  menuselect.makeopts

# Continue with a standard make.
RUN make 1> /dev/null
RUN make install 1> /dev/null
RUN make samples 1> /dev/null
WORKDIR /

# Copy in default configs
COPY http.conf /etc/asterisk/http.conf

CMD asterisk -f