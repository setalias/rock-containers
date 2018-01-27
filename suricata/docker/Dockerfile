# Dockerfile for Zookeeper

FROM rocknsm/base:latest
MAINTAINER Grant Curell <grant.curell@salientcrgt.com>

LABEL io.rocknsm.suricata="0.0.1"

COPY rocknsm-2.1.repo /etc/yum.repos.d/rocknsm-2.1.repo

# Add confd as a standard way to render configs
RUN yum -y install epel-release; yum -y update; \
    yum install -y confd; \
    mkdir -p /etc/confd/{conf.d,templates}; \
    rm -rf /var/cache/yum/*;










ENV ROCKNSM_ZOOKEEPER_USER=zookeeper \
    ROCKNSM_ZOOKEEPER_MYID=1 \
    ROCKNSM_ZOOKEEPER_CONFDIR=/etc/zookeeper \
    ROCKNSM_ZOOKEEPER_DATADIR=/var/lib/zookeeper/data \
    ROCKNSM_ZOOKEEPER_DATALOGDIR=/var/lib/zookeeper/logs \
    ROCKNSM_ZOOKEEPER_PORT=2181 \
    ROCKNSM_ZOOKEEPER_TICKTIME=2000 \
    ROCKNSM_ZOOKEEPER_INITLIMIT=10 \
    ROCKNSM_ZOOKEEPER_SYNCLIMIT=5 \
    ROCKNSM_ZOOKEEPER_MAXCLIENTCNXNS=60 \
    ROCKNSM_ZOOKEEPER_SERVERS=server.1=0.0.0.0:2888:3888

# Prepare the environment and get things up to date
RUN yum update -y; \
  yum install -y zookeeper java-1.8.0-openjdk-headless; \
  rm -rf /var/cache/yum/*; \
  mkdir -p "$ROCKNSM_ZOOKEEPER_DATADIR" \
           "$ROCKNSM_ZOOKEEPER_DATALOGDIR"; \
  chown -R "$ROCKNSM_ZOOKEEPER_USER:$ROCKNSM_ZOOKEEPER_USER" \
           "$ROCKNSM_ZOOKEEPER_DATADIR" \
           "$ROCKNSM_ZOOKEEPER_DATALOGDIR"; \
  systemctl enable zookeeper;

# Add configuration templates
COPY zoo.cfg.toml /etc/confd/conf.d/
COPY zoo_myid.toml /etc/confd/conf.d/
COPY zoo.cfg.tmpl /etc/confd/templates/
COPY zoo_myid.tmpl /etc/confd/templates/


# Expose data dirs
VOLUME ["$ROCKNSM_ZOOKEEPER_DATADIR", "$ROCKNSM_ZOOKEEPER_DATALOGDIR"]
EXPOSE $ROCKNSM_ZOOKEEPER_PORT 2888 3888

# systemd starts by default (from parent)
