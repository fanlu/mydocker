FROM ubuntu:16.04
MAINTAINER Fanlu <fanlu@jd.com>

RUN apt-get update && apt-get install -y openssh-server build-essential git cmake libopenblas-dev libopencv-dev graphviz supervisor vim wget unzip python-setuptools python-pip
RUN mkdir /var/run/sshd
RUN echo 'root:screencast' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
#RUN echo "export VISIBLE=now" >> /etc/profile

RUN groupadd -r admin && useradd -r -m -g admin admin && mkdir -p /export/App/work && chown -R admin:admin /export/App/work

# install the core library
COPY supervisord.conf /etc/supervisor/supervisord.conf
# python pakcage
RUN pip install numpy https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.12.0-cp27-none-linux_x86_64.whl matplotlib scikit-learn scikit-image pandas graphviz bokeh keras jupyter
USER admin
WORKDIR /export/App/work/
RUN git clone --recursive https://github.com/dmlc/mxnet/ && cd mxnet && \
	cp make/config.mk . && \
	echo "USE_BLAS=openblas" >>config.mk && \
	make -j$(nproc)
ENV PYTHONPATH /export/App/work/mxnet/python


#RUN mkdir -p /home/admin && chown -R admin:admin /home/admin
RUN jupyter notebook --generate-config && \
	echo "c.NotebookApp.ip = '*'" >> /home/admin/.jupyter/jupyter_notebook_config.py && \
	echo "c.NotebookApp.notebook_dir = u'/export/App/work/'" >> /home/admin/.jupyter/jupyter_notebook_config.py && \
	echo "c.NotebookApp.open_browser = False" >> /home/admin/.jupyter/jupyter_notebook_config.py && \
	echo "c.NotebookApp.password = u'sha1:d88a12643b69:40a1a3c636534e3c75f4ebb6c77cddbecc8b7562'" >> /home/admin/.jupyter/jupyter_notebook_config.py

USER root
RUN echo "export PYTHONPATH=/export/App/work/mxnet/python:$PYTHONPATH" >> /etc/profile
EXPOSE 22 8888
#CMD ["/usr/sbin/sshd", "-D"]
CMD ["/usr/bin/supervisord"]

