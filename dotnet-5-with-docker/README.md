

```
mkdir -p /tmp/dotnet-5-docker

docker run -it --rm `
  -p 5000:5000 `
  -v /tmp/dotnet-5-docker:/src `
  mcr.microsoft.com/dotnet/sdk:5.0
```

Now you have a .NET 5 development environment running, using your local drive for the source code.

_Inside the container:_

```
dotnet --list-sdks

dotnet new webapi \
  -o /src/api \
  --no-openapi --no-https

export ASPNETCORE_URLS=http://+:5000

dotnet run \
  --no-launch-profile \
  --project /src/api/api.csproj
```

Browse to http://localhost:5000/weatherforecast

```
ctrl-c

exit
```

_Build a Docker image:_

```
ls /tmp/dotnet-5-docker/api

cd /tmp/dotnet-5-docker

curl -o Dockerfile https://raw.githubusercontent.com/sixeyed/blog/master/dotnet-5-with-docker/Dockerfile

docker build -t dotnet-api:5.0 .
```

_Try the app:_

```
docker run -d -p 8010:80 --name api dotnet-api:5.0

curl http://localhost:8010/weatherforecast

docker logs api

docker rm -f api
```