IMG_NAME = brokers-ui
IMG = $(DOCKER_PUSH_REPOSITORY)$(DOCKER_PUSH_DIRECTORY)/$(IMG_NAME)
TAG = $(DOCKER_TAG)

ci-pr: resolve build validate test build-image push-image
ci-master: resolve build validate test build-image push-image
ci-release: resolve build validate test build-image push-image

resolve:
	npm install

build:
	../scripts/prepare-react-components.sh -d react-components
	npm run build

validate:
	eslint -c .eslinterrc ./src

test:
	CI=true npm run test

scan:
	sed -i -e 's/APIKEY/$(API_KEY)/g; s/USERKEY/$(USER_KEY)/g' /whitesource.config.json && cp /whitesource.config.json ./whitesource.config.json
	whitesource run

build-image:
	docker build -t $(IMG_NAME) .

push-image:
	docker tag $(IMG_NAME) $(IMG):$(TAG)
	docker push $(IMG):$(TAG)
