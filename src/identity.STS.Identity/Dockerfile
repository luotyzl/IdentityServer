FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/identity.STS.Identity/identity.STS.Identity.csproj", "src/identity.STS.Identity/"]
COPY ["src/identity.Admin.EntityFramework.Shared/identity.Admin.EntityFramework.Shared.csproj", "src/identity.Admin.EntityFramework.Shared/"]
COPY ["src/identity.Shared/identity.Shared.csproj", "src/identity.Shared/"]
RUN dotnet restore "src/identity.STS.Identity/identity.STS.Identity.csproj"
COPY . .
WORKDIR "/src/src/identity.STS.Identity"
RUN dotnet build "identity.STS.Identity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "identity.STS.Identity.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "identity.STS.Identity.dll"]