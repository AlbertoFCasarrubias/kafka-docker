# Kafka Docker for Render (Free Tier)

This project contains a self-hosted Kafka deployment **aggressively optimized** for Render's free tier.

⚠️ **Important**: This is for **testing only**.

## 🚀 Quick Deploy

1. **Connect to Render**:

   - Push this `kafka-docker` project to a GitHub repository
   - Connect the repo to Render

2. **Deploy**:

   - Use the `render.yaml` configuration
   - **Free tier compatible** (but expect limitations)

3. **Access**:
   - Service will be available at the provided Render URL
   - Internal apps can use the service name

## 📋 Configuration (Free Tier Optimizations)

### Memory Settings

- **Heap**: 200MB max, 100MB initial (very aggressive)
- **Total container**: ~400MB usage target

### Storage Limits

- **Retention**: Only 24 hours (1 day)
- **Max per topic**: 100MB
- **Segments**: 50MB each

### Performance Expectations

- ⚠️ **Limited throughput**
- ⚠️ **Possible container restarts**
- ⚠️ **Data loss after 24 hours**
- ✅ **Good for testing/learning**

## 🔧 Troubleshooting

### Container Still Exits?

- **Cause**: Even 200MB might be too much for free tier
- **Try**: Lower `KAFKA_HEAP_OPTS` to `-Xmx150m -Xms75m`
- **Alternative**: Use managed Kafka services

### InvalidReceiveException Errors?

- **Fixed**: Removed aggressive socket restrictions
- **Reason**: KRaft controller needs large message support (~1.2GB)
- **Solution**: Uses Kafka's default socket settings now

### Performance Issues

- **Expected**: Free tier has CPU/memory limits
- **Workaround**: Keep message sizes small (<1KB)

### Memory Warnings

- **Normal**: Kafka will show memory pressure warnings
- **Action**: Monitor container restart frequency

## 💰 Cost vs Alternatives

### Free Tier (Current)

- **Cost**: $0
- **Reliability**: ⚠️ Limited
- **Use**: Testing only

### Starter Plan Alternative

- **Cost**: ~$7/month
- **Reliability**: ✅ Much better
- **Memory**: 512MB → much more stable

### Managed Services

- **Confluent Cloud**: Free tier available
- **AWS MSK**: Pay per use
- **Aiven**: Free trial available

## 🔗 Integration

Connect your apps using the Render service URL:

```
KAFKA_BOOTSTRAP_SERVERS=your-kafka-service.onrender.com:443
```

For internal Render services:

```
KAFKA_BOOTSTRAP_SERVERS=kafka:9092
```

## ⚡ Quick Test

Once deployed, test with:

```bash
# Check if Kafka is responding
curl -I your-kafka-service.onrender.com
```

**Remember**: This is a **test setup**. For any real workload, consider managed Kafka services!
