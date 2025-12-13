# ------------------------------------------------------------------------------ 
# Etapa de build (instala dependencias y, si existiera, compila) 
# ------------------------------------------------------------------------------ 
    
FROM node:18-alpine AS builder
WORKDIR /app

# Copiamos sólo los manifiestos primero para aprovechar la cache de capas
COPY package.json yarn.lock ./

# Instalamos dependencias - sólo producción porque la app no tiene paso de build
RUN yarn install --frozen-lockfile --production=true && yarn cache clean

# Copiamos el resto de archivos de aplicación
COPY . .

# ------------------------------------------------------------------------------ 
# Etapa final: imagen runtime mínima
# ------------------------------------------------------------------------------ 

FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

# Copia solo lo necesario desde el builder
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/src ./src
COPY --from=builder /app/package.json ./package.json

EXPOSE 3000

# Ejecuta la app
CMD ["node", "src/index.js"]