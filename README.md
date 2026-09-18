DURANGO CUSTOM SERVER + CLIENT
INSTALLATION AND GITHUB GUIDE
=============================

This file is plain text and can be opened with Windows Notepad.

## DISCLAIMER

This is an independent community project created for research,
experimentation, development, and local/LAN testing.

This project is not affiliated with, sponsored by, authorized by,
or endorsed by Nexon.

All trademarks, game names, logos, and other intellectual property
remain the property of their respective owners.

Users are responsible for ensuring that their use and distribution
of the software, files, and assets comply with applicable laws,
licenses, and the rights of the respective intellectual property
owners.

Do not redistribute proprietary files, copyrighted assets, account
credentials, or other materials unless you have the right or
permission to distribute them.

This guide does not grant any license or permission to use third-party
intellectual property.

1. PACKAGE CONTENTS

---

SERVER PACKAGE:

GamonServer
startserver.bat
publish-server
DurangoServer.exe
Core
GameCode
Support
Shims
data
admin
and other required server files

```
AppData-nx\
```

CLIENT PACKAGE:

GamonClient
Durango.exe
UnityPlayer.dll
Durango_Data
locales
DinoWorldLauncher.exe
clusters.json

The package intentionally does not include old player saves, account
keys, logs, mods, obj files, private admin tokens, or other private data.

2. REQUIREMENTS

---

Server computer:
Windows 10/11 64-bit
.NET 9 SDK is required when rebuilding the server from source.
Internet access may be required for the first NuGet restore/build.

Client computer:
Windows 10/11 64-bit
No .NET SDK is required to run the client executable.

Install .NET 9 SDK from:

https://dotnet.microsoft.com/download/dotnet/9.0

3. INSTALL THE SERVER

---

1. Copy the complete GamonServer folder to a permanent location.

   Example:

   C:\DurangoServer\GamonServer

2. Make sure the folder structure looks like this:

   GamonServer
   startserver.bat
   publish-server
   DurangoServer.exe
   data
   admin
   and other required server files

3. You do NOT need to manually create AppData-nx.

   The startserver.bat script creates it automatically if it does not exist.

4. The resulting structure will be:

   GamonServer
   startserver.bat
   AppData-nx
   publish-server
   DurangoServer.exe

IMPORTANT:

AppData-nx contains private server/player save data.

Keep AppData-nx outside the GitHub repository when possible.

Do not upload player save files or private server data to a public
GitHub repository.

4. START THE SERVER

---

The server is started using:

GamonServer\startserver.bat

Double-click:

startserver.bat

The script automatically runs:

publish-server\DurangoServer.exe

Current server configuration:

Cluster key: nx
Gateway port: 8190
Game port: 8191
Maximum players: 200
Save root: GamonServer\AppData-nx

The server save directory is:

GamonServer\AppData-nx\

Keep this directory if you want to preserve player/world data.

IMPORTANT:

The admin token inside startserver.bat must remain private.

Do not upload the real admin token to GitHub.

Default ports:

Gateway HTTP: 8190
Game TCP:     8191

5. BUILDING THE SERVER FROM SOURCE

---

If you are working with the server source code instead of the
prebuilt GamonServer package:

1. Open PowerShell in the server source folder.

2. Restore packages:

   dotnet restore DurangoServer.csproj

3. Build:

   dotnet build DurangoServer.csproj -c Release

4. Create a publish folder:

   dotnet publish DurangoServer.csproj -c Release -o publish-server

5. Copy/update the resulting files into:

   GamonServer\publish-server\

Do NOT overwrite:

GamonServer\AppData-nx\

when updating the server.

6. LAN SETUP

---

For LAN multiplayer, the server and clients must be able to communicate
over the local network.

ON THE SERVER COMPUTER:

1. Start:

   GamonServer\startserver.bat

2. Find the server LAN IPv4 address:

   ipconfig

Example:

192.168.1.50

3. Allow these ports through Windows Firewall:

   TCP 8190
   TCP 8191

4. The server must be configured to accept LAN connections.

   If the server is only bound to:

   127.0.0.1

   other computers may not be able to connect.

ON EVERY CLIENT COMPUTER:

1. Extract the GamonClient package.

2. Open:

   clusters.json

3. Find:

   "gateway_url_root": "http://127.0.0.1:8190"

4. Replace 127.0.0.1 with the server LAN IPv4 address.

Example:

"gateway_url_root": "http://192.168.1.50:8190"

5. Keep port 8190.

6. Start:

   DinoWorldLauncher.exe

The game server TCP port 8191 must also be reachable from the clients.

TEST LAN CONNECTION:

From a client computer:

Test-NetConnection 192.168.1.50 -Port 8190

and:

Test-NetConnection 192.168.1.50 -Port 8191

A successful connection should show:

TcpTestSucceeded : True

7. INSTALL THE CLIENT

---

1. Copy/extract the complete GamonClient folder to the player computer.

   Example:

   C:\DurangoClient\GamonClient

2. Keep these files and folders together:

   Durango.exe
   UnityPlayer.dll
   Durango_Data
   locales
   DinoWorldLauncher.exe
   clusters.json

3. Edit:

   clusters.json

For a local server:

"gateway_url_root": "http://127.0.0.1:8190"

For a LAN server:

"gateway_url_root": "http://192.168.1.50:8190"

Replace 192.168.1.50 with the actual server LAN IP address.

4. Start:

   DinoWorldLauncher.exe

5. Select the configured server and press Play.

Do not start Durango.exe directly when using the launcher flow.

8. ADMIN WEB UI

---

When the server is running locally, open:

http://127.0.0.1:8190/admin/

From another LAN computer:

http://192.168.1.50:8190/admin/

Use the admin token configured in:

GamonServer\startserver.bat

IMPORTANT:

Never publish the admin token in:

* GitHub
* screenshots
* logs
* Discord
* chat
* documentation

If the token is exposed, replace/rotate it.

9. TROUBLESHOOTING

---

SERVER DOES NOT START:

* Confirm GamonServer\publish-server\DurangoServer.exe exists.
* Confirm startserver.bat is inside GamonServer.
* Check the server console for the actual error.
* Make sure required server files are present.

APPDATA-NX:

* It does not need to be created manually.
* startserver.bat creates it if necessary.
* Do not delete it if you want to keep player/world data.
* Back it up before updating the server.

HTTP CONNECTION REFUSED:

* Confirm the server process is running.
* Confirm gateway port 8190 is open.
* Confirm clusters.json uses the correct server IP.
* For LAN, make sure the server accepts connections from the LAN.

GAME CONNECTION FAILS AFTER HTTP WORKS:

* Confirm TCP port 8191 is open in Windows Firewall.
* Confirm the server uses --game-port 8191.
* Confirm the client can reach the server computer.

LAUNCHER SHOWS THE WRONG SERVER:

* Check clusters.json.
* Confirm gateway_url_root is correct.
* Restart DinoWorldLauncher.exe after changing clusters.json.

CHARACTERS OR WORLD DATA ARE MISSING:

* Confirm the server uses:

  GamonServer\AppData-nx\

* Make sure you are starting the same server installation.

* Do not randomly copy AppData between different server installations.

10. GITHUB - CREATE A PRIVATE REPOSITORY

---

Recommended:

Use a PRIVATE GitHub repository for server source and development
files.

Do not upload:

account keys
player saves
AppData-nx
admin tokens
passwords
private keys
logs
personal data

Create a repository at:

https://github.com/new

Set visibility to:

Private

Install Git for Windows if needed:

https://git-scm.com/download/win

Open PowerShell in the repository folder:

cd C:\Path\To\Gamon-CustomServer-main

Initialize Git:

git init

Add files:

git add .

Commit:

git commit -m "Initial Gamon Custom Server"

Connect the GitHub repository:

git branch -M main
git remote add origin https://github.com/OWNER/REPOSITORY.git

Push:

git push -u origin main

11. RECOMMENDED .GITIGNORE

---

Before the first GitHub push, create a .gitignore file containing
at least:

bin/
obj/
AppData/
AppData-nx/
logs/
*.log
*.player
*.player.bak
account.key
accountkey
*.token
*.secret

Also exclude:

admin tokens
passwords
private keys
credentials
personal player data

12. BEFORE EVERY GITHUB PUSH

---

Check that these are NOT included:

AppData-nx
AppData
account.key
accountkey
*.player
*.player.bak
logs
*.log
private admin tokens
passwords
private keys
credentials
personal data

If a secret was pushed accidentally:

* Revoke or rotate it immediately.
* Removing the file in a later commit is not enough.
* Remove the secret from Git history using a trusted secret-removal
  procedure.

13. SERVER PACKAGE DISTRIBUTION

---

The recommended downloadable packages are:

GamonServer.zip
GamonClient.zip

Server package:

GamonServer.zip

Client package:

GamonClient.zip

For large ZIP files, GitHub Releases are recommended instead of storing
large binary files directly inside the Git repository.

Example release:

Durango Custom Server v0.4

Files:

GamonServer.zip
GamonClient.zip

14. UPDATING THE SERVER

---

Before updating:

* Stop the running server.

* Back up:

  GamonServer\AppData-nx\

* Keep the backup separate from the source repository.

Then:

* Replace/update files inside:

  GamonServer\publish-server\

* Do NOT delete or overwrite:

  GamonServer\AppData-nx\

* Start:

  GamonServer\startserver.bat

The private save directory and the server executable are intentionally
kept separate.

15. FINAL FOLDER STRUCTURE

---

SERVER:

GamonServer
|
+-- startserver.bat
|
+-- AppData-nx
|     +-- private server/player save data
|
+-- publish-server
+-- DurangoServer.exe
+-- Core
+-- GameCode
+-- Support
+-- Shims
+-- data
+-- admin
+-- other required server files

CLIENT:

GamonClient
|
+-- DinoWorldLauncher.exe
+-- Durango.exe
+-- UnityPlayer.dll
+-- Durango_Data
+-- locales
+-- clusters.json

END OF GUIDE
