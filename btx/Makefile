IMG_NAME=btxcore_build
NO_CACHE=false

.PHONY: clean

all:
	docker build --no-cache=$(NO_CACHE) --rm -t "$(IMG_NAME)" .
	docker run --name="$(IMG_NAME)" -d "$(IMG_NAME)" /bin/true
	docker export btxcore_build | tar --wildcards -x 'root/*.deb'
	docker rm btxcore_build
	docker rmi btxcore_build
	mv ./root/*.deb ./
	rmdir ./root

clean:
	rm *.deb
