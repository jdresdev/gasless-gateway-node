# gasless-gateway-node

API Gateway de la plataforma **Gasless Vault**: un relayer de meta-transacciones EIP-712 para transferencias ERC-20 sin gas para el usuario final.

## Rol en la arquitectura

Punto de entrada HTTP del sistema. Genera los datos tipados EIP-712 listos para firmar, valida criptográficamente las firmas off-chain (sin RPC), encola las tareas de relay en Redis y expone una Query API que lee las transferencias confirmadas/pendientes desde PostgreSQL.

## Stack

- Node.js 20 + TypeScript
- Fastify
- `viem` (cliente público EVM y verificación de firmas)
- `ioredis` (cola de tareas)
- `zod` (validación de esquemas)

## Entregables previstos

- `GET /api/v1/vault/typed-data` — devuelve la estructura EIP-712 lista para firmar.
- `POST /api/v1/relayer/dispatch` — valida la firma y encola la tarea en `queue:relayer:tasks` (HTTP 202 + `taskId`).
- `GET /api/v1/transfers/:address` — consulta transferencias desde PostgreSQL.
