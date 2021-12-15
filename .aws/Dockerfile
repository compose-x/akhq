
ARG JAVA_VERSION=11
ARG BASE_IMAGE=public.ecr.aws/ews-network/amazoncorretto:${JAVA_VERSION}

FROM $BASE_IMAGE
RUN yum upgrade -y --security --exclude="*corretto*";                                                            \
    yum install -y shadow-utils ;                                               \
    groupadd -r akhq -g 1042 &&                                                  \
    useradd -u 1042 -r -g akhq -m -d /app -s /sbin/nologin -c "AKHQ user" akhq &&  \
    chown -R akhq: /app &&                                                       \
    yum erase shadow-utils -y && yum clean all && rm -rfv /var/cache/yum


ARG AKHQ_JAR
ENV AKHQ_JAR $AKHQ_JAR

COPY docker/usr /usr
COPY --chown=akhq:akhq docker/app /app
COPY --chown=akhq:akhq $AKHQ_JAR /app/akhq.jar

ENV MICRONAUT_CONFIG_FILES=/app/application.yml
USER akhq
ENV PATH=/usr/local/bin:$PATH
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["/app/akhq"]
