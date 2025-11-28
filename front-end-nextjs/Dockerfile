#Install dependencies
FROM node:22-slim AS deps
WORKDIR /app
COPY package*.json ./
RUN npm install

#Build the app
FROM node:22-slim AS build
WORKDIR /app

#dependencies installed is stored in node_modules directory 
#and we need the dependencies for the subsequent stages
COPY --from=deps /app/node_modules ./node_modules

#copy the content of the front-end-nextjs so we can build the app
COPY . .
RUN npm run build

#Production image
FROM node:22-slim AS runner
WORKDIR /app

#listen on port 3000 on all interfaces in the container
ENV HOST=0.0.0.0
ENV PORT=3000

#compiled nextjs app is stored in next directory
#The public folder contains static assets like images, fonts, and CSS that are referenced at runtime.
#installed dependencies is needed during runtime
COPY --from=build /app/.next ./.next
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/public ./public
COPY package*.json ./

EXPOSE 3000

CMD ["npm", "start"]