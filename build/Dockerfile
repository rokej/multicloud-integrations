FROM registry.ci.openshift.org/stolostron/builder:go1.17-linux AS builder

WORKDIR /go/src/github.com/open-cluster-management-io/multicloud-integrations
COPY . .
RUN make -f Makefile build

FROM registry.access.redhat.com/ubi8/ubi-minimal:latest

RUN microdnf update && \
     microdnf clean all

ENV OPERATOR=/usr/local/bin/multicloud-integrations \
    USER_UID=1001 \
    USER_NAME=multicloud-integrations

# install operator binary
COPY --from=builder /go/src/github.com/open-cluster-management-io/multicloud-integrations/build/_output/bin/gitopscluster /usr/local/bin/gitopscluster

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}
