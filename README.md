## Run

```
git clone git@github.com:haproxy/haproxy.git
cd haproxy

docker run --rm -it -v $(pwd):/usr/src/haproxy vkill/haproxy2-dev:alpine sh
> eval "make -j 2 all $makeOpts"
> vtest reg-tests/converter/sha2.vtc
```

## Ref

https://github.com/haproxy/haproxy/blob/master/doc/regression-testing.txt

https://github.com/docker-library/haproxy/blob/master/2.0/alpine/Dockerfile

## Docker build

```
docker build -t vkill/haproxy2-dev:alpine -f alpine/Dockerfile alpine
```
