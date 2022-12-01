FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

ENV ASPNETCORE_URLS=http://+:80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Kubernetes.App/Kubernetes.App.csproj", "Kubernetes.App/"]
RUN dotnet restore "Kubernetes.App/Kubernetes.App.csproj"
COPY . .
WORKDIR "/src/Kubernetes.App"
RUN dotnet build "Kubernetes.App.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Kubernetes.App.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Kubernetes.App.dll"]
