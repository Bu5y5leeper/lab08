## Homework 

1. Dockerfile
```sh
FROM ubuntu:latest

RUN apt-get update && apt-get install -yy gcc g++ cmake


COPY . solver/
WORKDIR solver/solver_application


RUN cmake -B _build && cmake --build _build

VOLUME /home/logs

ENTRYPOINT echo '1\n2\n3\n' | ./_build/main >> /home/logs/log.txt

```
3. yaml file
```sh
name: tp-lab-actions
run-name: ${{ github.actor }} is running TPLab04
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v4
      - name: Build and run in Docker container
        run: docker build -t solver_v1 ${{github.workspace}}
      - name: Run tests
        run: docker run -v ${{github.workspace}}/logs:/home/logs solver_v1
      - name: Check container
        run: cat ${{github.workspace}}/logs/log.txt
```

```
Copyright (c) 2015-2021 The ISC Authors
```
