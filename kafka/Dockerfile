FROM bitnami/kafka:latest

# KRaft mode configuration (no Zookeeper needed)
ENV KAFKA_CFG_NODE_ID=1
ENV KAFKA_CFG_PROCESS_ROLES=controller,broker
ENV KAFKA_CFG_CONTROLLER_QUORUM_VOTERS=1@localhost:9093
ENV KAFKA_CFG_LISTENERS=PLAINTEXT://0.0.0.0:9092,CONTROLLER://0.0.0.0:9093
ENV KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092
ENV KAFKA_CFG_LISTENER_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
ENV KAFKA_CFG_CONTROLLER_LISTENER_NAMES=CONTROLLER
ENV KAFKA_CFG_INTER_BROKER_LISTENER_NAME=PLAINTEXT

# Allow plaintext for testing
ENV ALLOW_PLAINTEXT_LISTENER=yes

# Aggressive memory settings for free tier (512MB total)
ENV KAFKA_HEAP_OPTS="-Xmx200m -Xms100m"

# Auto-create topics
ENV KAFKA_CFG_AUTO_CREATE_TOPICS_ENABLE=true

# Aggressive optimizations for free tier
ENV KAFKA_CFG_LOG_RETENTION_HOURS=24
ENV KAFKA_CFG_LOG_SEGMENT_BYTES=52428800
ENV KAFKA_CFG_LOG_RETENTION_BYTES=104857600
ENV KAFKA_CFG_NUM_PARTITIONS=1
ENV KAFKA_CFG_DEFAULT_REPLICATION_FACTOR=1

# Network settings - Removed socket restrictions for KRaft compatibility
ENV KAFKA_CFG_LOG_FLUSH_INTERVAL_MESSAGES=1000
ENV KAFKA_CFG_LOG_FLUSH_INTERVAL_MS=500
# Removed socket buffer restrictions that were causing controller issues
# KRaft mode needs default (larger) socket settings for controller operations

# Single-node optimizations
ENV KAFKA_CFG_OFFSETS_TOPIC_REPLICATION_FACTOR=1
ENV KAFKA_CFG_TRANSACTION_STATE_LOG_REPLICATION_FACTOR=1
ENV KAFKA_CFG_TRANSACTION_STATE_LOG_MIN_ISR=1

# Reduce background tasks for free tier
ENV KAFKA_CFG_LOG_CLEANUP_INTERVAL_MS=60000
ENV KAFKA_CFG_LOG_RETENTION_CHECK_INTERVAL_MS=60000
ENV KAFKA_CFG_REPLICA_FETCH_BACKOFF_MS=500

# JVM optimizations for low memory
ENV KAFKA_OPTS="-XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -Djava.awt.headless=true"

EXPOSE 9092

# Simple healthcheck for free tier
HEALTHCHECK --interval=60s --timeout=15s --start-period=120s --retries=2 \
    CMD netstat -an | grep 9092 | grep LISTEN || exit 1
