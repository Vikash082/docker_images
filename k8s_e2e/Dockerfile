FROM ubuntu:18.04

RUN apt update -y && apt upgrade -y

RUN apt install build-essential git curl rsync -y

RUN curl -s https://storage.googleapis.com/golang/go1.14.4.linux-amd64.tar.gz | tar -C /usr/local -xvz

ENV PATH=$PATH:/usr/local/go/bin

RUN go version && git clone https://github.com/kubernetes/kubernetes.git

RUN cd kubernetes && \
        git checkout release-1.17 && \
        make WHAT=test/e2e/e2e.test

RUN cd && mkdir e2ebin && mkdir -p ~/.kube/ && mkdir logs && \
        cp /kubernetes/_output/local/bin/linux/amd64/e2e.test e2ebin/

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl -o /kubectl \
        && chmod +x kubectl \
        && mv kubectl /usr/local/bin/

ENV KUBECONFIG=/root/.kube/config

CMD /root/e2ebin/e2e.test
