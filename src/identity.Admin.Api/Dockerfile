FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/identity.Admin.Api/identity.Admin.Api.csproj", "src/identity.Admin.Api/"]
COPY ["src/identity.Admin.EntityFramework.Shared/identity.Admin.EntityFramework.Shared.csproj", "src/identity.Admin.EntityFramework.Shared/"]
COPY ["src/identity.Admin.EntityFramework.SqlServer/identity.Admin.EntityFramework.SqlServer.csproj", "src/identity.Admin.EntityFramework.SqlServer/"]
COPY ["src/identity.Admin.EntityFramework.PostgreSQL/identity.Admin.EntityFramework.PostgreSQL.csproj", "src/identity.Admin.EntityFramework.PostgreSQL/"]
COPY ["src/identity.Shared/identity.Shared.csproj", "src/identity.Shared/"]
COPY ["src/identity.Admin.EntityFramework.MySql/identity.Admin.EntityFramework.MySql.csproj", "src/identity.Admin.EntityFramework.MySql/"]
RUN dotnet restore "src/identity.Admin.Api/identity.Admin.Api.csproj"
COPY . .
WORKDIR "/src/src/identity.Admin.Api"
RUN dotnet build "identity.Admin.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "identity.Admin.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "identity.Admin.Api.dll"]