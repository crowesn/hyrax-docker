FROM centos/ruby-27-centos7
RUN whoami
USER root

RUN yum install epel-release -y
RUN yum update -y && \
    yum install -y \
    redis \
    mysql \
    nodejs \
    unzip \
    ghostscript \
    ImageMagick \
    ImageMagick-devel \
    mediainfo \
    file \
    clamav \
    libclamav-dev \
    libreoffice \
    java-1.8.0-openjdk \
    java-1.8.0-openjdk-devel 

# Install ffmpeg
RUN cd /opt && wget https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-i686-static.tar.xz
RUN mkdir /opt/ffmpeg-git-i686-static && tar -xf /opt/ffmpeg-git-i686-static.tar.xz -C /opt/ffmpeg-git-i686-static --strip-components 1
RUN ln -s /opt/ffmpeg-git-i686-static/ffmpeg /usr/bin/ffmpeg

# Install Harvard's FITS tool
ENV FITS_VERSION 1.0.5
RUN curl -L http://projects.iq.harvard.edu/files/fits/files/fits-${FITS_VERSION}.zip \
      > /tmp/fits-${FITS_VERSION}.zip && \
  unzip /tmp/fits-${FITS_VERSION}.zip -d /opt/ && \
  rm -rf /tmp/fits-${FITS_VERSION}.zip && \
  ln -s /opt/fits-${FITS_VERSION} /opt/fits
COPY fits /opt/fits-${FITS_VERSION}
RUN chmod 755 /opt/fits-${FITS_VERSION}/fits.sh

# Create/Update the clamav database.
# TODO: is this necessary, it is very slow.
# RUN /usr/bin/freshclam --quiet

# Setup the app directory.
ENV APP_DIR=/opt/hyrax \
    TEMP=/opt/hyrax/tmp
RUN mkdir -p ${APP_DIR}
WORKDIR ${APP_DIR}

# Setup directory for ruby gems. This is to improve performance of the container.
ENV BUNDLE_GEMFILE=$APP_DIR/Gemfile \
    BUNDLE_JOBS=2 \
    BUNDLE_PATH=/opt/bundle
VOLUME ${BUNDLE_PATH}

EXPOSE 3000

COPY scripts /scripts

RUN chmod 700 /scripts/*.sh

ENTRYPOINT ["/scripts/docker-entrypoint.sh"]
CMD ["server"]
