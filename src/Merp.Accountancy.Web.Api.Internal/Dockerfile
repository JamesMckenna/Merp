#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/Merp.Accountancy.Web.Api.Internal/Merp.Accountancy.Web.Api.Internal.csproj", "src/Merp.Accountancy.Web.Api.Internal/"]
COPY ["src/Merp.Accountancy.Web.Core/Merp.Accountancy.Web.Core.csproj", "src/Merp.Accountancy.Web.Core/"]
COPY ["src/Merp.Accountancy.CommandStack/Merp.Accountancy.CommandStack.csproj", "src/Merp.Accountancy.CommandStack/"]
COPY ["src/Merp.Domain/Merp.Domain.csproj", "src/Merp.Domain/"]
COPY ["src/Merp.Web/Merp.Web.csproj", "src/Merp.Web/"]
COPY ["src/Merp.Accountancy.QueryStack/Merp.Accountancy.QueryStack.csproj", "src/Merp.Accountancy.QueryStack/"]
COPY ["src/Merp.Accountancy.Drafts/Merp.Accountancy.Drafts.csproj", "src/Merp.Accountancy.Drafts/"]
COPY ["src/Merp.Accountancy.Settings/Merp.Accountancy.Settings.csproj", "src/Merp.Accountancy.Settings/"]
RUN dotnet restore "src/Merp.Accountancy.Web.Api.Internal/Merp.Accountancy.Web.Api.Internal.csproj"
COPY . .
WORKDIR "/src/src/Merp.Accountancy.Web.Api.Internal"
RUN dotnet build "Merp.Accountancy.Web.Api.Internal.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Merp.Accountancy.Web.Api.Internal.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Merp.Accountancy.Web.Api.Internal.dll"]