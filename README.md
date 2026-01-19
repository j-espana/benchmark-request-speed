# API Speed Comparison: FastAPI vs Gin vs Actix-web

Tres proyectos idénticos implementados en diferentes frameworks para comparar su rendimiento.

## Proyectos

1. **FastAPI (Python)** - Puerto 8000
2. **Gin (Go)** - Puerto 8001
3. **Actix-web (Rust)** - Puerto 8002

Cada proyecto expone el endpoint `/api/data` que retorna un JSON con 5 usuarios.

## Instalación y Ejecución

### 1. FastAPI (Python)

```bash
cd fastapi-project

# Crear entorno virtual (opcional pero recomendado)
python3 -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar servidor
python main.py
```

Servidor corriendo en: http://localhost:8000/api/data

### 2. Gin (Go)

```bash
cd gin-project

# Descargar dependencias
go mod download

# Ejecutar servidor
go run main.go
```

Servidor corriendo en: http://localhost:8001/api/data

### 3. Actix-web (Rust)

```bash
cd actix-project

# Compilar y ejecutar
cargo run --release
```

Servidor corriendo en: http://localhost:8002/api/data

## Pruebas de Rendimiento

### Opción 1: Apache Bench (ab)

```bash
# Instalar ab (viene con Apache en macOS/Linux)
# Ubuntu/Debian: sudo apt-get install apache2-utils
# macOS: viene preinstalado

# Probar FastAPI
ab -n 10000 -c 100 http://localhost:8000/api/data

# Probar Gin
ab -n 10000 -c 100 http://localhost:8001/api/data

# Probar Actix-web
ab -n 10000 -c 100 http://localhost:8002/api/data
```

### Opción 2: wrk (Recomendado)

```bash
# Instalar wrk
# macOS: brew install wrk
# Ubuntu: sudo apt-get install wrk

# Probar FastAPI (30 segundos, 4 threads, 100 conexiones)
wrk -t4 -c100 -d30s http://localhost:8000/api/data

# Probar Gin
wrk -t4 -c100 -d30s http://localhost:8001/api/data

# Probar Actix-web
wrk -t4 -c100 -d30s http://localhost:8002/api/data
```

### Opción 3: autocannon (Node.js)

```bash
# Instalar autocannon
npm install -g autocannon

# Probar FastAPI
autocannon -c 100 -d 30 http://localhost:8000/api/data

# Probar Gin
autocannon -c 100 -d 30 http://localhost:8001/api/data

# Probar Actix-web
autocannon -c 100 -d 30 http://localhost:8002/api/data
```

### Opción 4: hey

```bash
# Instalar hey
# macOS: brew install hey
# Go: go install github.com/rakyll/hey@latest

# Probar FastAPI
hey -n 10000 -c 100 http://localhost:8000/api/data

# Probar Gin
hey -n 10000 -c 100 http://localhost:8001/api/data

# Probar Actix-web
hey -n 10000 -c 100 http://localhost:8002/api/data
```

## Script de Prueba Automatizado

Puedes crear un script para probar todos los servidores automáticamente:

```bash
#!/bin/bash

echo "=== Benchmark FastAPI (Puerto 8000) ==="
wrk -t4 -c100 -d10s http://localhost:8000/api/data

echo ""
echo "=== Benchmark Gin (Puerto 8001) ==="
wrk -t4 -c100 -d10s http://localhost:8001/api/data

echo ""
echo "=== Benchmark Actix-web (Puerto 8002) ==="
wrk -t4 -c100 -d10s http://localhost:8002/api/data
```

Guarda este script como `benchmark.sh`, dale permisos de ejecución (`chmod +x benchmark.sh`) y ejecútalo.

## Métricas a Comparar

- **Requests/sec**: Cantidad de peticiones por segundo
- **Latency**: Tiempo de respuesta promedio
- **Transfer/sec**: Cantidad de datos transferidos por segundo
- **Error rate**: Tasa de errores (idealmente 0%)

## Resultados Esperados (Aproximados)

Basado en benchmarks típicos:

1. **Actix-web (Rust)**: ~80,000-120,000 req/s
2. **Gin (Go)**: ~50,000-80,000 req/s
3. **FastAPI (Python)**: ~5,000-15,000 req/s

*Nota: Los resultados reales dependerán de tu hardware, sistema operativo y otras aplicaciones ejecutándose.*

## Verificar Endpoints

Antes de hacer benchmarks, verifica que todos los servidores respondan correctamente:

```bash
# FastAPI
curl http://localhost:8000/api/data

# Gin
curl http://localhost:8001/api/data

# Actix-web
curl http://localhost:8002/api/data
```

Todos deberían retornar el mismo JSON con la lista de usuarios.
