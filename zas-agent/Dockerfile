FROM python:2.7

ENV ZAS_SOURCE_URL=https://github.com/vulogov/zas_agent/archive/master.zip
# ENV ZAS_SOURCE_URL=https://github.com/alexanderzobnin/zas_agent/archive/numpy-dependency.zip
ENV ZAS_ARC_NAME=zas_agent-master
# ENV ZAS_ARC_NAME=zas_agent-numpy-dependency
ENV ZAS_ARC_FILE=${ZAS_ARC_NAME}.zip
ENV ZAS_WORKDIR="/zas-agent"

RUN apt-get update && apt-get install -y ca-certificates
RUN apt-get install -y unzip wget

# Download and extract
WORKDIR ${ZAS_WORKDIR}
RUN wget ${ZAS_SOURCE_URL} -O ${ZAS_ARC_FILE}
RUN unzip ${ZAS_ARC_FILE}

# Install zas_agent
WORKDIR ${ZAS_WORKDIR}/${ZAS_ARC_NAME}/install
RUN python ./check_python_packages.py
WORKDIR ${ZAS_WORKDIR}/${ZAS_ARC_NAME}
RUN python setup.py build
RUN python setup.py install

COPY ./run_zas_agent.sh ${ZAS_WORKDIR}/${ZAS_ARC_NAME}/run_zas_agent.sh

# Make port 10050 available to the world outside this container
EXPOSE 10050

# Set default redis port to connect
ENV REDIS_PORT=6379
ENV REDIS_HOST=redis

# Run zas_agent.py when the container launches
# ENTRYPOINT  ["./run_zas_agent.sh"]
CMD ["./run_zas_agent.sh"]