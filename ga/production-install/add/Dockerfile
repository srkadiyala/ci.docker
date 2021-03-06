# (C) Copyright IBM Corporation 2015, 2016.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:16.04

MAINTAINER Kavitha Suresh Kumar <kavisuresh@in.ibm.com>

RUN apt-get update \
    && apt-get install -y wget \
    && rm -rf /var/lib/apt/lists/*

# Install JRE
ADD ibm-java*archive.bin /tmp/java.bin

RUN chmod +x /tmp/java.bin \
   && /tmp/java.bin -i silent -DUSER_INSTALL_DIR=/opt/ibm/java \
   && rm /tmp/java.bin

ENV JAVA_HOME /opt/ibm/java
ENV PATH $JAVA_HOME/jre/bin:$PATH

# Install Liberty
ADD wlp-*-runtime-*.jar /tmp/wlp-runtime.jar

RUN java -jar /tmp/wlp-runtime.jar --acceptLicense /opt/ibm \
    && rm /tmp/wlp-runtime.jar

ENV PATH /opt/ibm/wlp/bin:$PATH

# Set Path Shortcuts
ENV LOG_DIR /logs
ENV WLP_OUTPUT_DIR /opt/ibm/wlp/output
RUN mkdir /logs \
    && ln -s $WLP_OUTPUT_DIR/defaultServer /output \
    && ln -s /opt/ibm/wlp/usr/servers/defaultServer /config

# Configure WebSphere Liberty
RUN /opt/ibm/wlp/bin/server create \
    && rm -rf $WLP_OUTPUT_DIR/.classCache

COPY server.xml /opt/ibm/wlp/usr/servers/defaultServer/

EXPOSE 9080 9443

CMD ["/opt/ibm/wlp/bin/server", "run", "defaultServer"]
