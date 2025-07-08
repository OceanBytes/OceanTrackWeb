FROM traccar/traccar:6.6-ubuntu

WORKDIR /opt/traccar

ARG ENVIRONMENT=dev
# Install tools needed to build the frontend
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    curl \
    nodejs \
    npm && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Clone and build the custom web UI
RUN git clone --branch ob_master https://github.com/OceanBytes/OceanTrackWeb.git /tmp/web-ui && \
    cd /tmp/web-ui && \
    npm install && \
    npm run build:$ENVIRONMENT && \
    rm -rf /opt/traccar/web/* && \
    cp -r /tmp/web-ui/build/* /opt/traccar/web/ && \
    rm -rf /tmp/web-ui

EXPOSE 8082 5055

# Start the server
ENTRYPOINT ["java", "-Xms1g", "-Xmx1g", "-Djava.net.preferIPv4Stack=true"]
CMD ["-jar", "tracker-server.jar", "conf/traccar.xml"]