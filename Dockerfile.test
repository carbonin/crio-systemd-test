FROM registry.access.redhat.com/ubi8/ubi-init
RUN dnf -y install httpd; dnf clean all; systemctl enable httpd
CMD [ "/sbin/init" ]
