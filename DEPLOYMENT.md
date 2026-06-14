# HireSense AI - Deployment Guide

## Deployment Options

### Option 1: Local Docker Deployment (Testing)

```bash
# Copy environment file
cp .env.example .env

# Edit .env with your actual values
nano .env

# Build and run with docker-compose
docker-compose up -d

# Check logs
docker-compose logs -f

# Stop services
docker-compose down
```

**Access:**
- Frontend (Streamlit): http://localhost:8501
- Backend (FastAPI): http://localhost:8000
- API Docs: http://localhost:8000/docs

---

### Option 2: Railway Deployment (Recommended for Production)

#### Prerequisites:
- Railway account (free at https://railway.app)
- GitHub repo connected (already done ✅)

#### Steps:

1. **Go to Railway Dashboard**
   - Create a new project
   - Select "Deploy from GitHub"
   - Choose `raju789p/HireSense_AI`

2. **Configure Services**

   **Backend Service:**
   - Dockerfile: `Dockerfile.backend`
   - Environment Variables:
     ```
     GROQ_API_KEY=your_key_here
     DATABASE_URL=mysql+pymysql://user:pass@host:3306/db
     PORT=8000
     ```

   **Frontend Service:**
   - Dockerfile: `Dockerfile.frontend`
   - Environment Variables:
     ```
     GROQ_API_KEY=your_key_here
     DATABASE_URL=mysql+pymysql://user:pass@host:3306/db
     BACKEND_URL=https://your-backend-domain.railway.app
     ```

   **Database Service:**
   - Add MySQL 8.0 from Railway marketplace
   - Generate random password

3. **Set Environment Variables in Railway**
   - Go to each service → Variables
   - Add all variables from `.env.example`

4. **Deploy**
   - Railway auto-deploys on push to main branch
   - Monitor deployments in Railway dashboard

5. **Get Your URLs**
   - Frontend URL: `https://your-app-name.railway.app`
   - Backend URL: `https://your-backend-name.railway.app`

---

### Option 3: Render Deployment (Alternative)

Similar to Railway but with slightly different setup. Instructions available on request.

---

## Health Checks

Backend has health endpoint:
```bash
curl http://localhost:8000/health
```

---

## Troubleshooting

### Database Connection Issues
```bash
# Check database logs
docker-compose logs db

# Verify connection
mysql -h localhost -u nlcs_user -p nlcs_db
```

### Backend Issues
```bash
# Check backend logs
docker-compose logs backend

# Run manually
python nlcs/backend/main.py
```

### Frontend Issues
```bash
# Check frontend logs
docker-compose logs frontend

# Run manually
streamlit run nlcs/app1.py
```

---

## Production Checklist

- [ ] Secure all API keys in environment variables
- [ ] Enable HTTPS/SSL
- [ ] Set up automated backups for database
- [ ] Configure CORS for frontend domain
- [ ] Set resource limits
- [ ] Enable monitoring/logging
- [ ] Test all API endpoints
- [ ] Load test the application

---

## Environment Variables Summary

| Variable | Purpose | Example |
|----------|---------|---------|
| `GROQ_API_KEY` | LLM API key | `gsk_xxx...` |
| `DB_HOST` | Database host | `db` (local) or Railway URL |
| `DB_USER` | Database user | `nlcs_user` |
| `DB_PASSWORD` | Database password | `secure_pass123` |
| `DB_NAME` | Database name | `nlcs_db` |
| `BACKEND_URL` | Backend API URL | `http://backend:8000` |
| `PORT` | FastAPI port | `8000` |

---

## Next Steps

1. **Test locally:** `docker-compose up`
2. **Fix any issues** before pushing
3. **Push to GitHub:** Changes auto-deploy
4. **Monitor on Railway/Render dashboard**

Good luck! 🚀
