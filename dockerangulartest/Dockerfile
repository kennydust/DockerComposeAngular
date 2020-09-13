FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
 
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build

#https://stackoverflow.com/questions/60833592/cannot-dockerize-a-net-core-3-1-worker-service-that-runs-on-os-x
#RUN mkdir -p /usr/local/share/dotnet/sdk/NuGetFallbackFolder
WORKDIR /src
COPY ["dockerangulartest.csproj", "./"]
#RUN dotnet restore "./dockerangulartest.csproj"
RUN dotnet restore 

COPY . .
WORKDIR "/src/."
RUN dotnet build "dockerangulartest.csproj" -c Release -o /app/build
RUN apt-get update && \
    apt-get install -y wget && \
    apt-get install -y gnupg2 && \
    wget -qO- https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y build-essential nodejs
     
FROM build AS publish
RUN dotnet publish "dockerangulartest.csproj" -c Release -o /app/publish
 
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dockerangulartest.dll"]