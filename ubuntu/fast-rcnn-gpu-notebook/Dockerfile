FROM karfai/caffe:gpu-jupyter
MAINTAINER Land007 <land_007@163.com>

#RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
#RUN echo "deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse\n\
#    deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse\n\
#    deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse\n\
#    deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse\n\
#    deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse\n\
#    deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse\n\
#    deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse\n\
#    deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse\n\
#    deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse\n\
#    deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse" > /etc/apt/sources.list

RUN apt-get update && apt-get install -y python-opencv

RUN cd /root && git clone --recursive https://github.com/rbgirshick/py-faster-rcnn.git
RUN cd /root/py-faster-rcnn/lib && make
ADD Makefile.config /root/py-faster-rcnn/caffe-fast-rcnn/
RUN cd /root/py-faster-rcnn/caffe-fast-rcnn && make -j8 && make pycaffe
RUN pip install easydict

#ADD https://people.eecs.berkeley.edu/~rbg/fast-rcnn-data/fast_rcnn_models.tgz /root/fast-rcnn/data/
#RUN cd /root/fast-rcnn/data/ && tar zxvf fast-rcnn-data/fast_rcnn_models.tgz
ADD fast_rcnn_models.tgz /root/fast-rcnn/data/

EXPOSE 8888

CMD echo 'Create your password for Jupyter Notebook' && \
    python -c "from IPython.lib import passwd; \
        f = open('hash.txt','w'); \
        f.write(passwd()); \
        f.close;" && \
    passwd="'`cat hash.txt`'" && \
    rm hash.txt && \
    \
    jupyter notebook \
        --ip='*' \
        --notebook-dir='/' \
        --no-browser \
        --NotebookApp.password=u${passwd}
