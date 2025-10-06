# Render Deployment Guide

This guide will help you deploy your chat application frontend to Render using Docker.

## 🚀 Quick Setup

### Step 1: Create Render Account
1. Go to [https://render.com](https://render.com)
2. Sign up with your GitHub account
3. Verify your email address

### Step 2: Connect GitHub Repository
1. In Render dashboard, click "New +"
2. Select "Web Service"
3. Connect your GitHub account if not already connected
4. Find and select your repository: `NguyenDucThuan-CS/minichatappfe`

### Step 3: Configure Web Service

#### Basic Settings:
- **Name**: `chatapp-frontend` (or your preferred name)
- **Environment**: `Docker`
- **Region**: `Oregon (US West)` (or closest to your users)
- **Branch**: `main`
- **Plan**: `Free` (for testing) or `Starter` (for production)

#### Docker Configuration:
- **Dockerfile Path**: `./Dockerfile`
- **Docker Context**: `.` (root directory)

#### Environment Variables:
Add these environment variables in Render dashboard:
```
NODE_ENV=production
NEXT_TELEMETRY_DISABLED=1
```

#### Advanced Settings:
- **Auto-Deploy**: ✅ Enabled (deploys automatically on main branch push)
- **Pull Request Previews**: ✅ Enabled (creates preview deployments for PRs)
- **Health Check Path**: `/` (or `/dashboard/default`)

### Step 4: Deploy
1. Click "Create Web Service"
2. Render will automatically:
   - Clone your repository
   - Build the Docker image
   - Deploy your application
   - Provide a public URL

## 🌐 Access Your Application

Once deployed, your app will be available at:
```
https://[your-service-name].onrender.com
```

For example: `https://chatapp-frontend.onrender.com`

## 📊 Monitoring & Management

### Render Dashboard Features:
- **Logs**: View real-time application logs
- **Metrics**: Monitor CPU, memory, and response times
- **Deployments**: View deployment history and status
- **Settings**: Modify environment variables and configuration

### Free Tier Limitations:
- **Sleep Mode**: App sleeps after 15 minutes of inactivity
- **Cold Start**: First request after sleep takes ~30 seconds
- **Bandwidth**: 100GB/month
- **Build Time**: 90 minutes/month

## 🔧 Custom Domain (Optional)

### Adding Custom Domain:
1. Go to your service settings
2. Click "Custom Domains"
3. Add your domain (e.g., `chat.yourdomain.com`)
4. Update DNS records as instructed
5. Enable SSL certificate (automatic)

## 🚀 CI/CD Integration

Your GitHub Actions workflow is already configured to:
1. ✅ Build and test your application
2. ✅ Create Docker image
3. ✅ Push to GitHub Container Registry
4. ✅ Trigger Render deployment automatically

### Manual Deployment:
If you need to manually trigger a deployment:
1. Go to Render dashboard
2. Select your service
3. Click "Manual Deploy"
4. Choose branch and commit

## 🔍 Troubleshooting

### Common Issues:

#### 1. Build Failures:
- Check Dockerfile syntax
- Verify all dependencies are in package.json
- Check build logs in Render dashboard

#### 2. Application Not Starting:
- Verify PORT environment variable (Render sets this automatically)
- Check application logs for errors
- Ensure Next.js standalone build is working locally

#### 3. Slow Performance:
- Enable caching headers in Next.js
- Optimize images and assets
- Consider upgrading to paid plan for better performance

#### 4. Sleep Mode Issues:
- Free tier apps sleep after inactivity
- First request after sleep takes longer
- Consider upgrading to paid plan to avoid sleep mode

### Debug Commands:
```bash
# Test Docker build locally
docker build -t chatapp-fe .

# Test Docker run locally
docker run -p 3000:3000 chatapp-fe

# Check application logs
# (View in Render dashboard)
```

## 📈 Performance Optimization

### For Production:
1. **Upgrade Plan**: Consider Starter plan ($7/month) for:
   - No sleep mode
   - Better performance
   - More resources

2. **Caching**: Add caching headers in Next.js:
   ```javascript
   // next.config.ts
   const nextConfig = {
     async headers() {
       return [
         {
           source: '/static/:path*',
           headers: [
             {
               key: 'Cache-Control',
               value: 'public, max-age=31536000, immutable',
             },
           ],
         },
       ]
     },
   }
   ```

3. **CDN**: Use Render's built-in CDN for static assets

## 🔐 Security Considerations

### Environment Variables:
- Never commit sensitive data to repository
- Use Render's environment variable system
- Rotate API keys regularly

### HTTPS:
- SSL certificates are automatically provided
- Force HTTPS redirects in Next.js configuration

## 📱 Mobile Optimization

Your Next.js app is already mobile-responsive, but consider:
- Testing on various devices
- Optimizing images for mobile
- Using Next.js Image component for better performance

## 🎉 Success!

Once deployed, you'll have:
- ✅ A publicly accessible chat application
- ✅ Automatic deployments from GitHub
- ✅ SSL certificate included
- ✅ Monitoring and logging
- ✅ Easy scaling options

Your chat application is now live and ready for users! 🚀
