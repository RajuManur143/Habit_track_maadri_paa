# Deploy on Fly.io (FREE - Docker-Friendly)

Fly.io is perfect for containerized deployments with excellent free tier.

## Prerequisites
- GitHub account
- Free Fly.io account (fly.io)
- Fly CLI installed locally

## Quick Start

### 1. Install Fly CLI

**Windows (PowerShell):**
```powershell
powershell -Command "iwr https://fly.io/install.ps1 -useb | iex"
```

**macOS/Linux:**
```bash
curl -L https://fly.io/install.sh | sh
```

Verify installation:
```bash
flyctl version
```

### 2. Create Fly.io Account

```bash
flyctl auth signup
# or
flyctl auth login
```

### 3. Initialize Fly App

In your project directory:

```bash
cd C:\Users\Administrator\OneDrive\Desktop\Raju_habit_tracker
flyctl launch
```

Follow the prompts:
- App name: `habit-tracker` (or choose unique name)
- Region: Select closest to you
- Postgres? No (use SQLite for free)
- Redis? No

This creates `fly.toml` file

### 4. Set Environment Variables

```bash
flyctl secrets set FLASK_ENV=production
flyctl secrets set SECRET_KEY="your-generated-secret-key"
```

**Generate SECRET_KEY:**
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

### 5. Deploy!

```bash
flyctl deploy
```

Watch the deployment in your terminal!

### 6. Open Your App

```bash
flyctl open
```

Your app is now live at: `https://habit-tracker.fly.dev`

---

## Fly.toml Configuration

The generated `fly.toml` should look like:

```toml
app = "habit-tracker"
primary_region = "sjc"  # San Jose (adjust as needed)

[build]
  image = "your-username/habit-tracker:latest"

[env]
  FLASK_ENV = "production"

[http_service]
  internal_port = 5000
  force_https = true
  auto_stop_machines = true
  auto_start_machines = true

[[services]]
  protocol = "tcp"
  internal_port = 5000
  processes = ["app"]

  [services.concurrency]
    type = "connections"
    hard_limit = 1000
    soft_limit = 200
```

---

## Managing Your App

### View Logs
```bash
flyctl logs
```

### Check App Status
```bash
flyctl status
```

### Restart App
```bash
flyctl restart
```

### Update Secrets
```bash
flyctl secrets set VARIABLE_NAME=value
flyctl secrets list
```

---

## Updating Your App

### Option 1: Push from Local (Recommended)
```bash
# Make changes
git add .
git commit -m "Update message"

# Deploy
flyctl deploy
```

### Option 2: Deploy from GitHub
```bash
flyctl launch --image-label=main
```

---

## Database Persistence

✅ **Good news!** SQLite persists in Fly.io with persistent volumes.

To enable persistent volume:

```bash
flyctl volumes create habits_data --size 3
```

Update `fly.toml`:
```toml
[mounts]
  source = "habits_data"
  destination = "/app/data"
```

Update `appraju.py`:
```python
DATABASE_PATH = os.getenv('DATABASE_PATH', '/app/data/habits.db')
```

Deploy:
```bash
flyctl deploy
```

---

## Free Tier Benefits

| Feature | Free Tier |
|---------|-----------|
| Shared-cpu-1x 256MB VMs | 3 ✅ |
| Shared Postgres | ✅ |
| Volumes | 3 GB total ✅ |
| Egress | 160 GB/month ✅ |
| Auto-scaling | ✅ |
| Custom domains | ✅ |
| HTTPS | ✅ |

---

## Scaling (if needed)

Add more VMs:
```bash
flyctl scale count 3
```

Increase resources:
```bash
flyctl scale vm shared-cpu-2x
```

---

## Troubleshooting

### App won't start
```bash
flyctl logs
```
Check output for errors

### Database connection failed
- Verify DATABASE_PATH environment variable
- Check volume is mounted correctly
- Review logs: `flyctl logs`

### Port issues
- Verify internal_port in fly.toml is 5000
- Check EXPOSE statement in Dockerfile

### Out of free tier limits
- Upgrade to paid (pay-as-you-go)
- Or reduce app count/scale

---

## Next Steps

1. ✅ Install Fly CLI
2. ✅ Create account: `flyctl auth signup`
3. ✅ Initialize app: `flyctl launch`
4. ✅ Set secrets: `flyctl secrets set`
5. ✅ Deploy: `flyctl deploy`
6. 🎉 Visit your app!

---

**Perfect for Docker and scaling! 🚀**
