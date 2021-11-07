FROM jupyter/minimal-notebook

COPY image_data .

RUN conda install --quiet --yes \
    'pandas' \
    'matplotlib-base' && \
    conda clean --all -f -y 


