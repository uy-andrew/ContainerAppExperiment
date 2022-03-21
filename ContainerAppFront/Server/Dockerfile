#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["ContainerAppFront/Server/ContainerAppFront.Server.csproj", "ContainerAppFront/Server/"]
COPY ["ContainerAppFront/Client/ContainerAppFront.Client.csproj", "ContainerAppFront/Client/"]
COPY ["ContainerAppFront/Shared/ContainerAppFront.Shared.csproj", "ContainerAppFront/Shared/"]
RUN dotnet restore "ContainerAppFront/Server/ContainerAppFront.Server.csproj"
COPY . .
WORKDIR "/src/ContainerAppFront/Server"
RUN dotnet build "ContainerAppFront.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ContainerAppFront.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ContainerAppFront.Server.dll"]