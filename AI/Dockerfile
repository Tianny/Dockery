FROM nvidia/cuda:10.0-cudnn7-runtime-centos7
RUN yum install kde-l10n-Chinese -y
RUN yum install glibc-common -y
RUN localedef -c -f UTF-8 -i zh_CN zh_CN.utf8
RUN export LANG=zh_CN.UTF-8
RUN echo "export LANG=zh_CN.UTF-8" >> /etc/locale.conf
ENV LANG zh_CN.UTF-8
ENV LC_ALL zh_CN.UTF-8
RUN yum -y install libXext libSM libXrender
RUN yum -y install python3 && unlink /usr/bin/python && ln -s /usr/bin/python3 /usr/bin/python
RUN pip3 install --upgrade pip && pip3 install setuptools --upgrade
RUN pip3 install torch===1.2.0 torchvision===0.4.0 -f https://download.pytorch.org/whl/torch_stable.html && pip3 install tensorflow-gpu==1.14 && pip3 install keras==2.2.5 && pip3 install opencv-python==4.2.0.34 && pip3 install scikit-image==0.17.2 && pip3 install scikit-learn==0.23.2
