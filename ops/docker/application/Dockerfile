# Use official Node.js Alpine image for smaller size and security
FROM node:20-alpine

# Create a non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set working directory
WORKDIR /var/www/app

# Copy package files first for better caching
COPY package*.json ./

# Install dependencies as root, then change ownership
RUN if [ -f package-lock.json ]; then \
        npm ci --omit=dev && npm cache clean --force; \
    else \
        npm install --omit=dev && npm cache clean --force; \
    fi

# Copy application source code
COPY src/ ./src/

# Create necessary directories and set proper ownership
RUN mkdir -p public storage && \
    chown -R appuser:appgroup /var/www/app

# Switch to non-root user
USER appuser

# Expose the application port (FastCGI for nginx proxy)
EXPOSE 9000

# Add health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD node -e "const http = require('http'); const options = { port: process.env.PORT || 9000, timeout: 2000 }; const req = http.get('http://localhost:' + options.port + '/health', (res) => { process.exit(res.statusCode === 200 ? 0 : 1); }); req.on('error', () => { process.exit(1); }); req.setTimeout(options.timeout, () => { req.destroy(); process.exit(1); });"

# Set default environment
ENV NODE_ENV=production
ENV PORT=9000

# Start the application
CMD ["node", "src/index.js"]
