FROM centos

ENV MYPATH /usr/local

WORKDIR ${MYPATH}

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo ${MYPATH}
CMD echo '--end--'
CMD /bin/bash