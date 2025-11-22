# Habit Tracker Application

A Flask-based web application for tracking daily habits with a beautiful, responsive UI.

## Features

- ✨ Add, delete, and track habits
- 📅 Monthly calendar view with progress tracking
- 📊 Daily progress chart visualization
- 🎨 Color-coded habits with emoji support
- 📈 Streak tracking (current and best)
- 💾 SQLite database for persistent storage

## Prerequisites

- Python 3.8 or higher
- pip (Python package manager)

## Installation

1. **Clone or download the project**
   ```bash
   cd Raju_habit_tracker
   ```

2. **Create a virtual environment (recommended)**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## Configuration

The application can be configured using environment variables:

```bash
# Set Flask environment (development or production)
export FLASK_ENV=production

# Set the database path (optional, defaults to current directory)
export DATABASE_PATH=/path/to/habits.db

# Set the secret key (REQUIRED for production - generate a secure key)
export SECRET_KEY=your-secure-secret-key-here

# Set the port (optional, defaults to 5000)
export PORT=5000
```

### Generating a Secret Key

For production, generate a secure secret key:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

## Running the Application

### Development Mode
```bash
export FLASK_ENV=development
python appraju.py
```

### Production Mode
```bash
export FLASK_ENV=production
export SECRET_KEY=your-secure-secret-key
python appraju.py
```

Then open your browser and navigate to:
```
http://localhost:5000
```

## Deployment

### Using Gunicorn (Production Server)

1. **Install Gunicorn**
   ```bash
   pip install gunicorn
   ```

2. **Run with Gunicorn**
   ```bash
   export FLASK_ENV=production
   export SECRET_KEY=your-secure-secret-key
   gunicorn -w 4 -b 0.0.0.0:5000 appraju:app
   ```

### Using Docker

Create a `Dockerfile`:
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY appraju.py .
ENV FLASK_ENV=production
ENV PORT=5000
EXPOSE 5000
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "appraju:app"]
```

Build and run:
```bash
docker build -t habit-tracker .
docker run -e SECRET_KEY=your-key -p 5000:5000 habit-tracker
```

### Using systemd (Linux/Mac)

Create `/etc/systemd/system/habit-tracker.service`:
```ini
[Unit]
Description=Habit Tracker Application
After=network.target

[Service]
Type=notify
User=www-data
WorkingDirectory=/var/www/habit-tracker
Environment="FLASK_ENV=production"
Environment="SECRET_KEY=your-secure-key"
ExecStart=/var/www/habit-tracker/venv/bin/gunicorn -w 4 -b 127.0.0.1:5000 appraju:app
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable habit-tracker
sudo systemctl start habit-tracker
```

## API Endpoints

### GET `/`
Returns the main HTML page

### GET `/api/habits`
**Query Parameters:**
- `year` (int): Year for the month (default: current year)
- `month` (int): Month number 1-12 (default: current month)

**Response:**
```json
{
  "habits": [
    {
      "id": 1,
      "name": "Gym",
      "emoji": "💪",
      "color": "bg-blue-100",
      "current_streak": 5,
      "best_streak": 10
    }
  ],
  "completions": {
    "1-2025-11-20": true
  },
  "daily_stats": [
    {"day": 1, "percentage": 80.0}
  ]
}
```

### POST `/api/habits`
Create a new habit

**Request Body:**
```json
{
  "name": "Morning Run",
  "emoji": "🏃"
}
```

### DELETE `/api/habits/<habit_id>`
Delete a habit

### POST `/api/completions`
Toggle completion for a habit

**Request Body:**
```json
{
  "habit_id": 1,
  "date": "2025-11-20"
}
```

## Database

- **SQLite database** automatically created on first run
- **Sample habits** added on initial setup (can be deleted)
- **Indexes** created for optimal query performance

## Security Considerations

For production deployment:

1. ✅ Set `FLASK_ENV=production` to disable debug mode
2. ✅ Generate and set a strong `SECRET_KEY`
3. ✅ Use HTTPS with reverse proxy (nginx/Apache)
4. ✅ Run behind a production WSGI server (Gunicorn)
5. ✅ Regular database backups
6. ✅ Monitor application logs

## Troubleshooting

### Port Already in Use
```bash
# Use a different port
export PORT=8000
python appraju.py
```

### Database Locked Error
- Close other instances of the application
- Ensure no other process is using the database

### Import Errors
```bash
# Reinstall dependencies
pip install --force-reinstall -r requirements.txt
```

## License

This project is open source and available for personal use.

## Support

For issues or questions, please check the logs or contact the development team.
