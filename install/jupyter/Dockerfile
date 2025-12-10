FROM python:3.11-slim

RUN apt-get update && apt-get install -y \
    wget curl git openjdk-21-jdk build-essential && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    bash /tmp/miniconda.sh -b -p /opt/conda && \
    rm /tmp/miniconda.sh

ENV PATH="/opt/conda/bin:${PATH}"

RUN conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main && \
    conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r && \
    conda clean -afy

RUN conda install -y -c conda-forge "nodejs>=20"
RUN pip install --no-cache-dir jupyterlab

WORKDIR /app
RUN git clone https://github.com/Systems-Modeling/SysML-v2-Release.git && \
    cd SysML-v2-Release/install/jupyter && \
    chmod +x ./install.sh && \
    bash -c "source /opt/conda/etc/profile.d/conda.sh && conda activate base && ./install.sh && conda clean -afy"

EXPOSE 8888

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
