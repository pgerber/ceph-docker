# Ceph Demo AIO
#

FROM ubuntu:16.04
MAINTAINER Sébastien Han "seb@redhat.com"

ENV CEPH_VERSION kraken

ENV CEPH_DEMO_ACCESS_KEY access_key
ENV CEPH_DEMO_SECRET_KEY secret_key
ENV CEPH_DEMO_UID demo_uid
ENV MON_IP 127.0.0.1
ENV CEPH_PUBLIC_NETWORK 0.0.0.0/0


# Install Ceph and prerequisites
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y wget unzip uuid-runtime python-setuptools udev sharutils python3 s3cmd && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FE869A9 && \
    echo "deb http://ppa.launchpad.net/gluster/libntirpc/ubuntu xenial main" | tee /etc/apt/sources.list.d/libntirpc.list && \
    wget -q -O- 'https://download.ceph.com/keys/release.asc' | apt-key add - && \
    echo "deb http://download.ceph.com/debian-$CEPH_VERSION/ xenial main" | tee /etc/apt/sources.list.d/ceph-$CEPH_VERSION.list && \
    apt-get update && apt-get install -y --force-yes ceph-mon ceph-osd ceph-mds ceph-mgr ceph-base ceph-common radosgw rbd-mirror && \
    rm -rf /tmp/* /var/tmp/*

RUN apt-get install -y vim less

# Add s3cfg file
ADD s3cfg /root/.s3cfg

# Add entrypoint
ADD entrypoint.sh /entrypoint.sh

# Add volumes for Ceph config and data
VOLUME ["/etc/ceph","/var/lib/ceph"]

# Expose the Ceph ports
EXPOSE 6789 6800 6801 6802 6803 6804 6805 80 5000

# Execute the entrypoint
WORKDIR /
ENTRYPOINT ["/entrypoint.sh"]
