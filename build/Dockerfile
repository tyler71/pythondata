FROM python:3.10 as init

RUN apt-get update                               \
 && apt-get install -y sqlite3                   \
                       texlive-xetex             \
                       musl                      \
                       texlive-fonts-recommended \
                       texlive-plain-generic     \
                       nodejs                    \
                       npm                       \
                       pandoc                    \
                       jq                        \
                       ripgrep                   \
 && rm -r /var/lib/apt/lists/*
# musl = Golang


RUN useradd --system --create-home --home-dir /home/application --shell /bin/bash \
      --gid root --uid 1000 application

RUN python -m pip install --no-cache-dir --upgrade pip

#RUN python -m pip install --no-cache-dir \
#     jupyter_contrib_nbextensions        \
#     jupyter_nbextensions_configurator   \
#     jupyterthemes                       \
# && jupyter contrib nbextension install

# Javascript kernel
# RUN npm install -g ijavascript \
#  && ijsinstall --install=global --spec-path=full

# Go kernel
COPY --from=golang:1.16-alpine /usr/local/go/ /usr/local/go/
ENV PATH="/usr/local/go/bin:${PATH}"
ENV GOPATH="/go"
RUN ( \
  mkdir -p "$(go env GOPATH)"/src/github.com/gopherdata                                   \
  && cd "$(go env GOPATH)"/src/github.com/gopherdata                                      \
  && git clone https://github.com/gopherdata/gophernotes                                  \
  && cd gophernotes                                                                       \
  && git checkout -f v0.7.5                                                               \
  && go install                                                                           \
  && mkdir -p /home/application/.local/share/jupyter/kernels/gophernotes                  \
  && cp kernel/* /home/application/.local/share/jupyter/kernels/gophernotes               \
  && cd /home/application/.local/share/jupyter/kernels/gophernotes                        \
  && chmod +w ./kernel.json                                                               \
  && sed "s|gophernotes|$(go env GOPATH)/bin/gophernotes|" < kernel.json.in > kernel.json \
  )
RUN chown -R application /home/application

COPY ./requirements .
RUN python -m pip install --no-cache-dir -r requirements

RUN chown application -R /usr/local/share/jupyter/lab

USER application

#RUN mkdir -p $(jupyter --data-dir)/nbextensions \
# && cd $(jupyter --data-dir)/nbextensions       \
# && git clone --depth=1 https://github.com/lambdalisue/jupyter-vim-binding vim_binding

#RUN jupyter contrib nbextension install --user                        \
# && jupyter nbextensions_configurator enable --user                   \
# && jupyter nbextension enable vim_binding/vim_binding                \
# && python -m bash_kernel.install

#COPY files/vim_binding.css /home/application/.local/share/jupyter/nbextensions/vim_binding/vim_binding.css

WORKDIR /home/application/data

CMD ["jupyter-lab", "--no-browser", "--ip=0.0.0.0"]
