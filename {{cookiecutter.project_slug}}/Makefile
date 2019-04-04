SRC = {{cookiecutter.project_slug}}
TAG = {{cookiecutter.author_dockerhub}}/{{cookiecutter.project_slug}}:latest

requirements: ## create requirements.txt
	pipreqs --force --savepath requirements.txt $(SRC)

install: ## install requirements
	conda install --yes --file requirements.txt

install-dev: ## install development requirements
	conda install --yes --file requirements-dev.txt
	pip install safety

clean: ## remove Python file artifacts
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f {} +
	find . -name '__pycache__' -exec rm -fr {} +

lint: ## check style
	pylint $(SRC)
	pycodestyle --show-source $(SRC)
	pydocstyle $(SRC)
	safety check
	
format: # make style edits
	isort -rc $(SRC)
	yapf -ir --style pep8 $(SRC)

test: ## run tests

docker-build: ## build Docker image
	docker build --no-cache --rm --tag $(TAG) .

docker-run: ## create and enter Docker container
	docker run -it -p {{cookiecutter.host_port}}:{{cookiecutter.container_port}} $(TAG) bash

docker-clean: ## remove all unused images and built containers
	docker system prune -af

nb: ## start jupyter server
	jupyter notebook --ip 0.0.0.0 --no-browser --allow-root &
	echo 'Go to port {{cookiecutter.host_port}} on the host'
