FROM alpine
MAINTAINER MO

# Include dist
ADD dist/ /root/dist/

# Install packages
RUN apk -U upgrade && \
    apk add bash ca-certificates file wget && \
    apk -U add --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ \
            suricata && \

# Setup user, groups and configs
    mv /root/dist/suricata.yaml /etc/suricata/suricata.yaml && \

# Download the latest EmergingThreats ruleset
    wget --no-parent -l1 -r --no-directories -P /etc/suricata/rules/ https://rules.emergingthreats.net/open/suricata/rules/ && \

# Clean up
    rm -rf /root/* && \
    rm -rf /var/cache/apk/*

# Start suricata
CMD /bin/bash -c "exec suricata -i $(/sbin/ip address | grep '^2: ' | awk '{ print $2 }' | tr -d [:punct:])"
