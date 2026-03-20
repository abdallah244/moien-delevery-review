# Host Deploy (Hostinger VPS) — Moien Delivery

This document is a practical deploy checklist for the current Hostinger VPS setup.

## Production host

- VPS IP: `147.93.56.214`
- Web: `https://moiendelivery.lu`
- API: `https://api.moiendelivery.lu`

## Production paths (Hostinger htdocs layout)

- Web root (static Angular build): `/home/moiendelivery/htdocs/moiendelivery.lu`
- API root (NestJS project): `/home/moiendelivery-api/htdocs/api.moiendelivery.lu`

## Credentials (important)

Do **not** store real passwords inside this repository (even if it’s “not on GitHub”).

Use one of these safe options instead:

- Option A (recommended): set an environment variable locally:
  - `VPS_USER=root`
  - `VPS_HOST=147.93.56.214`
  - `VPS_PASS=1UH,Qay#qf/Zznfo(ek)`
- Option B: prompt for the password at runtime (PowerShell `Read-Host -AsSecureString`).
- Option C: store it in Windows Credential Manager and read it at runtime.

## Windows (PowerShell) tooling

These examples assume you have:

- Node.js installed locally
- `tar` available (Windows 10/11 usually includes it)
- Posh-SSH PowerShell module installed (`New-SSHSession`, `Set-SCPItem`, `Invoke-SSHCommand`)

## Deploy Web (Angular)

### 1) Build locally

From repo root:

```powershell
cd "c:\moien delevery\wfrontend"

# Production build
npx ng build --configuration production
```

### 2) Create a tarball from the build output

```powershell
cd "c:\moien delevery\wfrontend\dist\wfrontend\browser"
tar -czf "C:\moien delevery\frontend-deploy.tar.gz" *
```

### 3) Upload + extract on VPS

```powershell
$hostIp = $env:VPS_HOST
$user = $env:VPS_USER

# Option A: password via env var
$pw = ConvertTo-SecureString $env:VPS_PASS -AsPlainText -Force

# Option B: prompt (uncomment)
# $pw = Read-Host "VPS password" -AsSecureString

$cred = New-Object System.Management.Automation.PSCredential($user, $pw)

Set-SCPItem -ComputerName $hostIp -Credential $cred -Path "C:\moien delevery\frontend-deploy.tar.gz" -Destination "/tmp/" -Force -AcceptKey

$s = New-SSHSession -ComputerName $hostIp -Credential $cred -Force -AcceptKey
Invoke-SSHCommand -SSHSession $s -Command "ls -la /tmp/frontend-deploy.tar.gz && cd /home/moiendelivery/htdocs/moiendelivery.lu && rm -rf * && tar -xzf /tmp/frontend-deploy.tar.gz && chown -R moiendelivery:moiendelivery . && echo DEPLOYED"
Remove-SSHSession $s | Out-Null
```

## Deploy API (NestJS)

There are two typical strategies:

### Strategy A (recommended): build on the VPS

```bash
cd /home/moiendelivery-api/htdocs/api.moiendelivery.lu
source ~/.nvm/nvm.sh

git pull
npm ci
rm -rf dist
npm run build

pm2 restart moiendelivery-api
```

### Strategy B: upload changed files + build on VPS

If you changed a few TypeScript files locally, you can SCP them to the same path on the server (preserving folders), then run the build + PM2 restart.

## Uploading a small file when SCP fails (0 bytes workaround)

Sometimes SCP can silently upload an empty file. A reliable workaround is base64:

```powershell
# Example: upload a SQL file as base64
$bytes = [System.IO.File]::ReadAllBytes("C:\path\to\file.sql")
$b64 = [Convert]::ToBase64String($bytes)

$s = New-SSHSession -ComputerName $env:VPS_HOST -Credential $cred -Force -AcceptKey
Invoke-SSHCommand -SSHSession $s -Command "echo $b64 | base64 -d > /tmp/file.sql && wc -l /tmp/file.sql"
Remove-SSHSession $s | Out-Null
```

## Quick health checks

- API:
  - `curl -i https://api.moiendelivery.lu/api/v1/health`
- Web:
  - open `https://moiendelivery.lu`
