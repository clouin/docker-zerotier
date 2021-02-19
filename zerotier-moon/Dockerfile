FROM jerryin/zerotier

EXPOSE 9993/udp

COPY startup.sh /startup.sh
RUN chmod 0755 /startup.sh

ENTRYPOINT ["/startup.sh"]