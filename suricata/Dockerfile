FROM alpine
MAINTAINER MO

# Include dist
ADD dist/ /root/dist/

# Install packages
RUN apk -U upgrade && \
    apk add bash ca-certificates file procps wget && \
    apk -U add --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ \
            suricata && \

# Setup user, groups and configs
    addgroup -g 2000 suri && \
    adduser -S -H -u 2000 -D -g 2000 suri && \
    mv /root/dist/suricata.yaml /etc/suricata/suricata.yaml && \
    mv /root/dist/capture-filter.bpf /etc/suricata/capture-filter.bpf && \

# Download the latest EmergingThreats ruleset, replace rulebase and enable all rules
    cd /root/dist/ && \
    wget https://rules.emergingthreats.net/open/suricata-3.2/emerging.rules.tar.gz && \
    tar xvfz emerging.rules.tar.gz -C /etc/suricata/ && \
    sed -i s/^#alert/alert/ /etc/suricata/rules/*.rules && \

# Clean up
    apk del ca-certificates wget && \
    rm -rf /root/* && \
    rm -rf /var/cache/apk/*

# Start suricata
CMD suricata -v -F /etc/suricata/capture-filter.bpf -i $(/sbin/ip address | grep '^2: ' | awk '{ print $2 }' | tr -d [:punct:])
