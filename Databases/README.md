## How create a docker image from an existing database
1. Create .bacpac file from MSSMS
2. Copy the .bacpac file to the docker container
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=hereyourpassword" \
  -p 1433:1433 --name hereContainerName \
  -v ${PWD}:/var/opt/mssql/backups \
  -d mcr.microsoft.com/mssql/server:2022-latest
```
3. Install sqlpackage

```bash
dotnet tool install -g microsoft.sqlpackage
```
4. Copy the .bacpac file to the container
```bash
docker exec -it hereContainerName /opt/mssql-tools/bin/sqlpackage \
  /a:Import /sf:/var/opt/mssql/backups/yourdatabase.bacpac \
  /tdn:yourdatabasename /tu:sa /tp:hereyourpassword
```
5. If you want use from your local machine, you can use the following command, the -sf flag is the path to the .bacpac file:
```bash
sqlpackage -a:Import -tsn:localhost,1433 -tdn:DemoDatabase -tu:sa -tp:Saidleb3n. -sf:".bacpac" -TargetTrustServerCertificate:true
```