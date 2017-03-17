FROM phusion/baseimage

RUN rm -rf /etc/apt/sources.list.d/proposed.list
RUN sed -i -re 's/\w+\.archive\.ubuntu\.com/archive.ubuntu.com/g' /etc/apt/sources.list
RUN apt-get update && apt-get install -y gearman-job-server && rm -r /var/lib/apt/lists/*

VOLUME ["/var/www"]
WORKDIR /var/www

CMD "gearmand"
