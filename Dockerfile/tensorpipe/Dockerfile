FROM centos:latest
MAINTAINER wangyongliang <wangyongliang1.jd.com>
RUN pwd
# boost deps
ADD lib/* /lib64/
# tensorpipe
ADD bin/tensorpipe /usr/local/bin/tensorpipe
RUN yum install -y unzip

