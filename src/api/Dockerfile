#See https://aka.ms/containerfastmode to understand how Visual Studio use this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM dataimages123.azurecr.io/sdk:latest AS build
WORKDIR /src
COPY ["DataServices/DataServices.csproj", "DataServices/"]
RUN dotnet restore "DataServices/DataServices.csproj"
COPY . .
WORKDIR "/src/DataServices"
RUN dotnet build "DataServices.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DataServices.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DataServices.dll"]
