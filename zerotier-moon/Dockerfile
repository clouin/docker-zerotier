FROM jerryin/zerotier:1.6.5

EXPOSE 9993/udp

COPY startup.sh /startup.sh
RUN chmod 0755 /startup.sh

ENTRYPOINT ["/startup.sh"]