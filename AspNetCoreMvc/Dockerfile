#1. image
FROM mcr.microsoft.com/dotnet/sdk:7.0 as buildName
WORKDIR /app

#Sadece csproj dosyas�n� app i�ine kopyala eklememizin amac� bir controller ekledi�imizde tamamen ba�tan i�lem yapmas�n� istemedi�imizden kaynaklanmaktad�r. Cacheden gelecek. wwwrootta de�i�iklik oldu�unda projenin di�er dosyalar� tekrar olu�turulmayacak cacheden gelecektir.
COPY *.csproj .
RUN dotnet restore
#bununla birlikte statik bir dosya kopyalanacak.
COPY . . 
RUN  dotnet restore
RUN  dotnet publish AspNetCoreMvc.csproj -c Release -o out

#2.image
##a�a��daki ve yukar�daki imajlar ayr�labilir.
##yukar�da out i�ine publish edilmi� dosyalar� a�a��daki imagede ta��mam gerekiyor
#faydas� sdk deki b�y�k imagedan kurtulmu� oluyorum.
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
COPY --from=buildName /app/out .
ENV ASPNETCORE_URLS="http://*:4500"
ENV ASPNETCORE_ENVIRONMENT="DEVELOPMENT"
ENV MySqlCon="Uzak Sunucu Veritaban� Yolu"
ENTRYPOINT ["dotnet","AspNetCoreMvc.dll"]