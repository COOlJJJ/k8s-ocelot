FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["k8s-ocelot/k8s-ocelot.csproj", "k8s-ocelot/"]
RUN dotnet restore "k8s-ocelot/k8s-ocelot.csproj"
COPY . .
WORKDIR "/src/k8s-ocelot"
RUN dotnet build "k8s-ocelot.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "k8s-ocelot.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "k8s-ocelot.dll"]