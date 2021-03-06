FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY dotnetapp/*.csproj ./dotnetapp/
COPY utils/*.csproj ./utils/
COPY tests/*.csproj ./tests/
RUN dotnet restore

# copy everything else and build app
COPY . .
WORKDIR /app/dotnetapp
RUN dotnet build --no-restore


FROM build AS testrunner
WORKDIR /app/tests
ENTRYPOINT ["dotnet", "test", "--logger:trx"]


FROM build AS test
WORKDIR /app/tests
RUN dotnet test


FROM build AS publish
WORKDIR /app/dotnetapp
RUN dotnet publish -c Release -o out --no-restore


FROM mcr.microsoft.com/dotnet/runtime:5.0 AS runtime
WORKDIR /app
COPY --from=publish /app/dotnetapp/out ./
RUN ls -ltRr
RUN chmod -R 777 *
RUN ls -ltrR
ENTRYPOINT ["./dotnetapp"]
