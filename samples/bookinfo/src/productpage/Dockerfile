# Copyright Istio Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

FROM python:3.7.7-slim

WORKDIR /

COPY requirements.txt ./
RUN pip3 install -vvv --no-cache-dir -r requirements.txt

COPY test-requirements.txt ./
RUN pip3 install --no-cache-dir -r test-requirements.txt

COPY productpage.py /opt/microservices/
COPY tests/unit/* /opt/microservices/
COPY templates /opt/microservices/templates
COPY static /opt/microservices/static
COPY requirements.txt /opt/microservices/

ARG flood_factor
ENV FLOOD_FACTOR ${flood_factor:-0}

EXPOSE 9080
WORKDIR /opt/microservices

#install all dependencies to auto instrumentation
RUN opentelemetry-bootstrap -a install 
#RUN python -m unittest discover

ENV OTEL_SERVICE_NAME=productpage 
ENV OTEL_TRACES_EXPORTER=console 
ENV OTEL_METRICS_EXPORTER=console 
# ENV OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=0.0.0.0:4317
# opentelemetry-instrument
#CMD ["bash"]
CMD ["/usr/local/bin/opentelemetry-instrument", "python" "productpage.py","9080"]
# CMD ["python", "productpage.py", "9080"]
