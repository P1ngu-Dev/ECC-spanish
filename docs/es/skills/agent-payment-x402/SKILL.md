---
name: agent-payment-x402
description: Add x402 payment execution to AI agents with per-task budgets, spending controls, and non-custodial wallets. Supports Base through agentwallet-sdk and X Layer through OKX Payments / OKX Agent Payments Protocol.
origin: community
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->
# Ejecución de pago del agente (x402)

Permita que los agentes de IA realicen pagos sujetos a políticas con controles de gasto integrados. Utiliza el protocolo de pago HTTP x402 y herramientas MCP para que los agentes puedan pagar por servicios externos, API u otros agentes sin riesgo de custodia.

## Cuándo utilizar

Úselo cuando: su agente necesite pagar una llamada API, comprar un servicio, llegar a un acuerdo con otro agente, aplicar límites de gasto por tarea o administrar una billetera sin custodia. Se combina naturalmente con habilidades de revisión de seguridad y canalización de llm conscientes de los costos.

## Árbol de decisión

Elija la ruta de integración en función de si su agente compra acceso a una API paga o cobra a otros por una:

| Necesidad | Ruta recomendada |
|------|------------------|
| El agente paga una API 402 en Base u otra cadena compatible con la billetera del agente | Utilice `agentwallet-sdk` como servidor de pagos MCP con una estricta política de gastos |
| El agente paga una API con control 402 en X Layer | Utilice el protocolo de pagos del agente OKX de `okx/onchainos-skills`; `okx-x402-payment` es un alias heredado obsoleto |
| La API de TypeScript cobra a los agentes | Utilice los documentos SDK del vendedor TypeScript de OKX Payments para Express, Hono, Fastify o Next.js |
| Go API cobra a los agentes | Utilice los documentos SDK del vendedor de OKX Payments Go para Gin, Echo o `net/http` |
| Rust API cobra a los agentes | Utilice los documentos del SDK del vendedor de OKX Payments Rust para Axum |
| La API de Java cobra a los agentes | Utilice los documentos del SDK del vendedor de Java de OKX Payments para Spring Boot 2/3, Java EE o Jakarta |
| La API de Python cobra a los agentes | Verifique el repositorio actual de OKX Payments antes de la implementación; Es posible que una guía para vendedores de Python no esté disponible |

## Redes compatibles

- `agentwallet-sdk`: use los documentos del paquete para confirmar la cobertura de red actual antes de la producción. Base Sepolia es el desarrollo predeterminado más seguro; La red principal base es la ruta de producción indicada por la habilidad original.
- Pagos OKX/Capa X: los documentos del vendedor actuales apuntan a la Capa X (`eip155:196`) y la liquidación de USDT0. Obtenga los documentos SDK actuales antes de generar el código de producción porque los paquetes de pago y el comportamiento del facilitador pueden cambiar rápidamente.

## Cómo funciona

### Protocolo x402
x402 extiende HTTP 402 (pago requerido) a un flujo negociable por máquina. Cuando un servidor devuelve `402`, la herramienta de pago del agente negocia el precio, verifica el presupuesto, firma una transacción y vuelve a intentarlo solo dentro de los límites de política y confirmación establecidos por el orquestador.

### Controles de gastos
Cada llamada a una herramienta de pago aplica un `SpendingPolicy`:
- **Presupuesto por tarea**: gasto máximo para la acción de un solo agente
- **Presupuesto por sesión**: límite acumulativo en toda una sesión
- **Destinatarios incluidos**: restringe qué direcciones/servicios puede pagar el agente
- **Límites de velocidad**: transacciones máximas por minuto/hora

### Carteras sin custodia
Los agentes poseen sus propias claves a través de cuentas inteligentes ERC-4337. El orquestador establece la política antes que la delegación; el agente sólo puede gastar dentro de unos límites. Sin fondos mancomunados, sin riesgo de custodia.

## Integración MCP

La capa de pago expone herramientas MCP estándar que se adaptan a cualquier código Claude o configuración de arnés de agente.

> **Nota de seguridad**: Fije siempre la versión del paquete. Esta herramienta administra claves privadas: las instalaciones `npx` no fijadas introducen riesgos en la cadena de suministro.

### Opción A: agentwallet-sdk (Base/multicadena)

```json
{
  "mcpServers": {
    "agentpay": {
      "command": "npx",
      "args": ["agentwallet-sdk@6.0.0"]
    }
  }
}
```
### Herramientas disponibles (invocables por el agente)

| Herramienta | Propósito |
|------|---------|
| `get_balance` | Consultar el saldo de la billetera del agente |
| `send_payment` | Enviar pago a domicilio o ENS |
| `check_spending` | Consultar presupuesto restante |
| `list_transactions` | Seguimiento de auditoría de todos los pagos |

> **Nota**: La política de gastos la establece el **orquestador** antes de delegarla al agente, no el propio agente. Esto evita que los agentes aumenten sus propios límites de gasto. Configure la política a través de `set_policy` en su capa de orquestación o enlace previo a la tarea, nunca como una herramienta a la que pueda llamar el agente.

### Opción B: Protocolo de pagos del agente OKX (capa X)

Utilice esta ruta para X Layer x402, pago multipartito (MPP), pago de sesión, cargos y flujos de cargos A2A.

Para flujos de agentes del lado del comprador:

1. Instale o haga referencia al repositorio `okx/onchainos-skills` actual.
2. Utilice `skills/okx-agent-payments-protocol/SKILL.md` como despachador.
3. Trate a `skills/okx-x402-payment/SKILL.md` como un alias de compatibilidad obsoleto, no como la habilidad canónica.
4. Requerir confirmación explícita del usuario antes de verificar el estado de la billetera o realizar acciones de pago. No oculte la ejecución de pagos detrás de una llamada de herramienta genérica.

Para los flujos de API del lado del vendedor, obtenga la guía específica del idioma más reciente antes de generar el código:

| Tiempo de ejecución | Guía actual |
|---------|---------------|
| Mecanografiado | `https://raw.githubusercontent.com/okx/payments/main/typescript/SELLER.md` |
| Ir | `https://raw.githubusercontent.com/okx/payments/main/go/x402/SELLER.md` |
| Óxido | `https://raw.githubusercontent.com/okx/payments/main/rust/x402/SELLER.md` |
| Java | `https://raw.githubusercontent.com/okx/payments/main/java/SELLER.md` |

No copie ejemplos de documentos antiguos sin consultar el repositorio OKX actual. La guía actual de OKX utiliza `okx-agent-payments-protocol` como despachador y los documentos del vendedor de Java ya están disponibles.

## Ejemplos

### Aplicación del presupuesto en un cliente MCP

Al crear un orquestador que llame al servidor MCP de pago de agentes, aplique presupuestos antes de enviar llamadas a herramientas pagas.

> **Requisitos previos**: instale el paquete antes de agregar la configuración de MCP: `npx` sin `-y` solicitará confirmación en entornos no interactivos, lo que provocará que el servidor se cuelgue: `npm install -g agentwallet-sdk@6.0.0`

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";

async function main() {
  // 1. Validate credentials before constructing the transport.
  //    A missing key must fail immediately — never let the subprocess start without auth.
  const walletKey = process.env.WALLET_PRIVATE_KEY;
  if (!walletKey) {
    throw new Error("WALLET_PRIVATE_KEY is not set — refusing to start payment server");
  }

  // Connect to the agentpay MCP server via stdio transport.
  // Whitelist only the env vars the server needs — never forward all of process.env
  // to a third-party subprocess that manages private keys.
  const transport = new StdioClientTransport({
    command: "npx",
    args: ["agentwallet-sdk@6.0.0"],
    env: {
      PATH: process.env.PATH ?? "",
      NODE_ENV: process.env.NODE_ENV ?? "production",
      WALLET_PRIVATE_KEY: walletKey,
    },
  });
  const agentpay = new Client({ name: "orchestrator", version: "1.0.0" });
  await agentpay.connect(transport);

  // 2. Set spending policy before delegating to the agent.
  //    Always verify success — a silent failure means no controls are active.
  const policyResult = await agentpay.callTool({
    name: "set_policy",
    arguments: {
      per_task_budget: 0.50,
      per_session_budget: 5.00,
      allowlisted_recipients: ["api.example.com"],
    },
  });
  if (policyResult.isError) {
    throw new Error(
      `Failed to set spending policy — do not delegate: ${JSON.stringify(policyResult.content)}`
    );
  }

  // 3. Use preToolCheck before any paid action
  await preToolCheck(agentpay, 0.01);
}

// Pre-tool hook: fail-closed budget enforcement with four distinct error paths.
async function preToolCheck(agentpay: Client, apiCost: number): Promise<void> {
  // Path 1: Reject invalid input (NaN/Infinity bypass the < comparison)
  if (!Number.isFinite(apiCost) || apiCost < 0) {
    throw new Error(`Invalid apiCost: ${apiCost} — action blocked`);
  }

  // Path 2: Transport/connectivity failure
  let result;
  try {
    result = await agentpay.callTool({ name: "check_spending" });
  } catch (err) {
    throw new Error(`Payment service unreachable — action blocked: ${err}`);
  }

  // Path 3: Tool returned an error (e.g., auth failure, wallet not initialised)
  if (result.isError) {
    throw new Error(
      `check_spending failed — action blocked: ${JSON.stringify(result.content)}`
    );
  }

  // Path 4: Parse and validate the response shape
  let remaining: number;
  try {
    const parsed = JSON.parse(
      (result.content as Array<{ text: string }>)[0].text
    );
    if (!Number.isFinite(parsed?.remaining)) {
      throw new TypeError("missing or non-finite 'remaining' field");
    }
    remaining = parsed.remaining;
  } catch (err) {
    throw new Error(
      `check_spending returned unexpected format — action blocked: ${err}`
    );
  }

  // Path 5: Budget exceeded
  if (remaining < apiCost) {
    throw new Error(
      `Budget exceeded: need $${apiCost} but only $${remaining} remaining`
    );
  }
}

main().catch((err) => {
  console.error(err);
  process.exitCode = 1;
});
```
## Mejores prácticas

- **Establecer presupuestos antes de la delegación**: al generar subagentes, adjunte una Política de gasto a través de su capa de orquestación. Nunca le dé a un agente gastos ilimitados.
- **Fije sus dependencias**: especifique siempre una versión exacta en su configuración de MCP (por ejemplo, `agentwallet-sdk@6.0.0`). Verifique la integridad del paquete antes de implementarlo en producción.
- **Seguimientos de auditoría**: use `list_transactions` en enlaces posteriores a la tarea para registrar lo que se gastó y por qué.
- **Fallo cerrado**: si no se puede acceder a la herramienta de pago, bloquee la acción paga; no recurra al acceso no medido.
- **Emparejar con revisión de seguridad**: las herramientas de pago tienen altos privilegios. Aplique el mismo escrutinio que el acceso al shell.
- **Pruebe primero con redes de prueba**: utilice Base Sepolia para el desarrollo; cambiar a la red principal base para producción.

## Referencia de producción

- **npm**: [@@P0@@](https://www.npmjs.com/package/agentwallet-sdk)
- **Fusionado en NVIDIA NeMo Agent Toolkit**: [PR #17](https://github.com/NVIDIA/NeMo-Agent-Toolkit-Examples/pull/17) — herramienta de pago x402 para ejemplos de agentes de NVIDIA
- **Especificación de protocolo**: [x402.org](https://x402.org)
- **SDK de pagos de OKX**: [@@P1@@](https://github.com/okx/payments): integraciones de vendedor de TypeScript, Go, Rust y Java para X Layer x402
- **Habilidad del Protocolo de pagos del agente OKX**: [@@P2@@](https://github.com/okx/onchainos-skills/tree/main/skills/okx-agent-payments-protocol)
- **Resumen de pagos OKX**: [web3.okx.com/onchainos/dev-docs/payments/overview](https://web3.okx.com/onchainos/dev-docs/payments/overview)