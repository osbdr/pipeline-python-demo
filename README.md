# pipeline-python-demo

Beispiel einer Pipeline, die folgendes kann:
- Code Format mit `black` überprüfen
- Sortierung der Imports mit `isort` überprüfen
- Tests ausführen
- Erstellung eines Docker Images

```
stages:
- black
- isort
- test
- docker

black:
  stage: black
  image: python:3.8.2-slim
  before_script:
    - pip install -r requirements.txt 
  script:
    - black --check .

isort:
  stage: isort
  image: python:3.8.2-slim  
  before_script:
    - pip install -r requirements.txt 
  script:
    - isort *.py -c -vb

test:
  stage: test
  image: python:3.8.2-slim
  before_script:
    - pip install -r requirements.txt 
  script:
    - python -m unittest discover

docker:
  stage: docker
  image: docker:19.03.8
  variables:
    BUILD_ARGS: >
      --build-arg no_proxy=$no_proxy
      --build-arg http_proxy=$http_proxy
      --build-arg https_proxy=$https_proxy
  services:
    - docker:19.03.8-dind
  script:
    - docker build $BUILD_ARGS -t python-demo .
```

## Referenzen
- black: https://github.com/psf/black
- isort: https://github.com/timothycrosley/isort

- Docker Pipeline: https://partner.bdr.de/gitlab/kfe-devops/docker-demo
