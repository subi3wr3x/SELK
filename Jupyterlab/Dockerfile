FROM alpine:latest

RUN apk update \
    && apk --no-cache add g++ libffi-dev \
        linux-headers krb5-dev openssl-dev \
        py3-pip python3-dev

RUN pip3 install cython 
RUN pip3 install numpy
RUN pip3 install pandas xlrd 'prompt-toolkit<2.1.0' \
    sparkmagic jupyterlab


FROM alpine:latest  

RUN apk update \
    && apk --no-cache add python3 npm texlive-xetex texlive-full \
        krb5
COPY --from=0 /usr/lib/python3.8/site-packages /usr/lib/python3.8/site-packages
COPY --from=0 /usr/share/jupyter/lab /usr/share/jupyter/lab
COPY --from=0 /usr/bin/jupyter* /usr/bin/
RUN mkdir -p /opt/jupyter/notebooks/

RUN cd /usr/lib/python3.8/site-packages \
    && jupyter labextension install @jupyter-widgets/jupyterlab-manager \
    && jupyter-kernelspec install sparkmagic/kernels/pysparkkernel

EXPOSE 8888

ENTRYPOINT ["jupyter-lab", "--ip=*", "--port=8888", "--no-browser", "--notebook-dir=/opt/jupyter/notebooks", "--allow-root"]
