IMAGE_NAME=drlee001/k8s-mariadb-galera-centos
IMAGE_VERSION=v004-k8s113
#LOCAL_REGISTRY=localhost:5000

image:
	docker build -t $(IMAGE_NAME):$(IMAGE_VERSION) .

push:
	#docker tag $(IMAGE_NAME):$(IMAGE_VERSION)
	docker push $(IMAGE_NAME):$(IMAGE_VERSION)
