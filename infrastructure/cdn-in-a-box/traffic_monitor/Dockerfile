# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
############################################################
# Dockerfile to build Traffic Monitor container images
# Based on CentOS
############################################################

FROM centos/systemd

# Default values for RPM -- override with `docker build --build-arg RPM=...'
ARG RPM=traffic_monitor/traffic_monitor.rpm
ADD $RPM /

RUN yum install -y epel-release && \
    yum install -y \
        jq \
        nmap-ncat \
        iproute \
        net-tools \
        gettext \
        bind-utils \
        openssl \
        initscripts && \
    yum install -y  /$(basename $RPM) && \
    rm /$(basename $RPM) && \
    yum clean all

RUN mkdir -p /opt/traffic_monitor/conf
ADD traffic_monitor/traffic_monitor.cfg /opt/traffic_monitor/conf/

ADD enroller/server_template.json \
    traffic_monitor/parameters.json \
    traffic_monitor/profile.json \
    traffic_ops/to-access.sh \
    /

EXPOSE 80
ADD traffic_monitor/run.sh /
ENTRYPOINT /run.sh
