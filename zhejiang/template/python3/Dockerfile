FROM openfaas/classic-watchdog:0.18.1 as watchdog

FROM python:3.5.6-slim-stretch
COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN mkdir app
WORKDIR /home/app/

COPY index.py .
#COPY requirements.txt   .
RUN mkdir -p function
RUN touch ./function/__init__.py          .

COPY index.py           .
COPY requirements.txt   .
RUN mkdir -p function
RUN touch ./function/__init__.py
COPY function           function


RUN pip install -i http://pypi.douban.com/simple --trusted-host pypi.douban.com -r requirements.txt

ENV fprocess="python index.py"
EXPOSE 8080

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]
