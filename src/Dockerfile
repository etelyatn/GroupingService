FROM microsoft/dotnet:2.2-sdk-alpine as builder  
 
RUN mkdir -p /root/src/app/grouping-service
WORKDIR /root/src/app/grouping-service
 
COPY GroupingService.sln . 
COPY Api/Api.csproj ./Api/Api.csproj
COPY Models/Models.csproj ./Models/Models.csproj
COPY Repository/Repository.csproj ./Repository/Repository.csproj
COPY Services/Services.csproj ./Services/Services.csproj 
COPY Tests/Tests.csproj ./Tests/Tests.csproj 
RUN dotnet restore

COPY . .
RUN dotnet build
#RUN dotnet test Tests/Tests.csproj
RUN dotnet publish -c Release -o published Api/Api.csproj

FROM microsoft/dotnet:2.2-aspnetcore-runtime-alpine as dotnet-alpine

WORKDIR /root/  
COPY --from=builder /root/src/app/grouping-service/Api/published .
ENV ASPNETCORE_URLS=http://+:5000
EXPOSE 5000/tcp
CMD ["dotnet", "./Api.dll"]  