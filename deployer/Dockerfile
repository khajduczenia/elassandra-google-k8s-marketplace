FROM launcher.gcr.io/google/debian9 AS build

RUN apt-get update \
    && apt-get install -y --no-install-recommends gettext

ADD chart/elassandra /tmp/chart
RUN cd /tmp && tar -czvf /tmp/elassandra.tar.gz chart

ADD schema.yaml /tmp/schema.yaml

ARG REGISTRY
ARG TAG

RUN cat /tmp/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /tmp/schema.yaml.new \
    && mv /tmp/schema.yaml.new /tmp/schema.yaml

FROM gcr.io/cloud-marketplace-tools/k8s/deployer_helm
COPY --from=build /tmp/elassandra.tar.gz /data/chart/
COPY --from=build /tmp/schema.yaml /data/

