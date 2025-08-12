FROM node:20 AS build

# Install git
RUN apt-get update && apt-get install -y git

# Clone Piped frontend
RUN git clone https://github.com/TeamPiped/Piped-Frontend.git /app

WORKDIR /app

# Set backend URL (replace YOUR_BACKEND_URL with your backend Render URL)
ENV BACKEND_URL=https://y-v282.onrender.com
ENV PORT=3000
# below line giving error
# RUN sed -i "s|https://pipedapi.kavin.rocks|$BACKEND_URL|g" src/config.json

# Build frontend
RUN npm install && npm run build

# Serve with lightweight server
FROM node:20-alpine
WORKDIR /app

COPY --from=build /app/dist /app/dist

RUN npm install -g serve

EXPOSE 3000
CMD ["serve", "-s", "dist", "-l", "3000"]
