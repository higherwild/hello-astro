# Stage 1: Build
FROM node:lts-alpine AS build
WORKDIR /app

# Copy package files
COPY package*.json ./
RUN npm install

# Copy source
COPY . .

# Build with remote DB connection for schema generation
ARG ASTRO_DB_REMOTE_URL
ENV ASTRO_DB_REMOTE_URL=$ASTRO_DB_REMOTE_URL
RUN npm run build

# Stage 2: Runtime
FROM node:lts-alpine
WORKDIR /app

# Copy built artifacts
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/package.json ./

# Environment
ENV HOST=0.0.0.0
ENV PORT=4321
ENV NODE_ENV=production

# Run
EXPOSE 4321
CMD ["node", "./dist/server/entry.mjs"]
