# builder
FROM mcr.microsoft.com/dotnet/sdk:3.1-alpine as build

WORKDIR /src
COPY ./src/api/api.csproj .
RUN dotnet restore

COPY ./src/api/ .
RUN dotnet publish -c Release -o /out

# app
FROM mcr.microsoft.com/dotnet/aspnet:3.1-alpine

ENTRYPOINT ["dotnet", "/app/api.dll"]
WORKDIR /app
COPY --from=build /out/ /app