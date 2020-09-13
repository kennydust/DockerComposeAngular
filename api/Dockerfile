FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env

# https://stackoverflow.com/questions/60833592/cannot-dockerize-a-net-core-3-1-worker-service-that-runs-on-os-x
#RUN mkdir -p /usr/local/share/dotnet/sdk/NuGetFallbackFolder

WORKDIR /app
#COPY *.csproj ./

COPY core/*.csproj ./core/
RUN dotnet restore ./core/core.csproj

COPY apitest/*.csproj ./apitest/
RUN dotnet restore ./apitest/apitest.csproj

#copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out /app/apitest

#Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "apitest.dll"]