## Run

```
git clone git@github.com:haproxy/haproxy.git

docker run --rm -it -v $(pwd)/haproxy:/usr/src/haproxy vkill/haproxy2-dev:alpine sh

> eval "make -C /usr/src/haproxy -j 2 all $makeOpts"
> eval "make install-bin $makeOpts"

> HAPROXY_PROGRAM=/usr/local/sbin/haproxy vtest reg-tests/converter/sha2.vtc
```

## Ref

https://github.com/haproxy/haproxy/blob/master/doc/regression-testing.txt

https://github.com/docker-library/haproxy/blob/master/2.0/alpine/Dockerfile

## Docker build

```
docker build -t vkill/haproxy2-dev:alpine -f alpine/Dockerfile alpine
```
