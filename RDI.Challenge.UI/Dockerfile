#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["RDI.Challenge.UI/RDI.Challenge.UI.csproj", "RDI.Challenge.UI/"]
COPY ["RDI.Challenge.DataContext/RDI.Challenge.DataContext.csproj", "RDI.Challenge.DataContext/"]
COPY ["RDI.Challenge.Domain/RDI.Challenge.Domain.csproj", "RDI.Challenge.Domain/"]
COPY ["RDI.Challenge.Business/RDI.Challenge.Business.csproj", "RDI.Challenge.Business/"]
COPY ["RDI.Challenge.RabbitMQ/RDI.Challenge.RabbitMQ.csproj", "RDI.Challenge.RabbitMQ/"]
RUN dotnet restore "RDI.Challenge.UI/RDI.Challenge.UI.csproj"
COPY . .
WORKDIR "/src/RDI.Challenge.UI"
RUN dotnet build "RDI.Challenge.UI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "RDI.Challenge.UI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RDI.Challenge.UI.dll"]