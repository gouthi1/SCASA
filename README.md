first clone it "https://github.com/gouthi1/SCASA.git"
write Dockerfile:
FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /app
COPY . .
RUN dotnet restore "SCASA.csproj"
RUN dotnet build "SCASA.csproj" -c Release -o /app/build
RUN dotnet publish "SCASA.csproj" -c Release -o /app/publish
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 80
ENTRYPOINT ["dotnet", "SCASA.dll"]
Run the below commands: docker build -t image1 .
docker run -d -p 8000:80 --name gouthi image1
