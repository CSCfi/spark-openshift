FROM quay.io/uninett/jupyter-spark:20180901-0c7a087

MAINTAINER Apurva Nandan <apurva.nandan@csc.fi>

RUN pip install virtualenv

USER root

RUN mkdir /tmp/worker \
    && chmod -R 771 /tmp/worker

# 1. Openshift permissions
# 2. START_COMMAND is used to run either driver, master or worker 
RUN chmod ug+rw /etc/passwd \
    && echo 'echo "spark:x:$(id -u):$(id -g)::/tmp:/bin/bash" >> /etc/passwd' > /usr/local/bin/fix-username-and-start.sh \
    && echo '$START_COMMAND' >> /usr/local/bin/fix-username-and-start.sh

RUN chgrp root /usr/local/bin/fix-username-and-start.sh \
    && chmod 774 /usr/local/bin/fix-username-and-start.sh

RUN chgrp -R root /home/notebook/.cache/pip \
    && chmod -R 774 /home/notebook/.cache/pip

ADD start-notebook.sh /usr/local/bin/
ADD hadoop-openstack-3.0.0.jar /opt/jars/hadoop-os.jar
ADD hadoop-aws-2.7.3.jar /opt/jars/hadoop-aws.jar
ADD aws-java-sdk-1.7.4.jar /opt/jars/aws-java-sdk.jar

CMD ["/usr/local/bin/fix-username-and-start.sh"]
