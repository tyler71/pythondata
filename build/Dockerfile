FROM python

RUN apt-get update             \
 && apt-get install -y sqlite3 \
 && rm -r /var/lib/apt/lists/*

RUN useradd --system --create-home --home-dir /home/application --shell /bin/bash \
      --gid root --uid 1000 application


RUN pip install jupyter_contrib_nbextensions \
                jupyterthemes

COPY ./requirements .
RUN python -m pip install --no-cache-dir -r requirements

USER application

RUN mkdir -p $(jupyter --data-dir)/nbextensions \
 && cd $(jupyter --data-dir)/nbextensions \
 && git clone --depth=1 https://github.com/lambdalisue/jupyter-vim-binding vim_binding

RUN jupyter contrib nbextension install --user      \
 && jupyter nbextensions_configurator enable --user \
 && jupyter nbextension enable vim_binding/vim_binding

COPY files/vim_binding.css /home/application/.local/share/jupyter/nbextensions/vim_binding/vim_binding.css

WORKDIR /home/application/data

CMD ["jupyter-lab", "--no-browser", "--ip=0.0.0.0"]
