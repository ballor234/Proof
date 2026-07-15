# Mi Repositorio de Aprendizaje en Base

Este repo es mi espacio personal para documentar todo lo que vaya aprendiendo sobre Base (L2 de Ethereum).

**Objetivo principal:** Organizar conocimiento y práctica mientras exploro el ecosistema.

### Recursos iniciales recomendados

- [Documentación oficial](https://docs.base.org)
- [Build on Base](https://base.org/build)
- [Bridge oficial](https://bridge.base.org)
- [Blockscout - Explorador](https://base.blockscout.com)

Estos son los enlaces básicos con los que estoy empezando.

### Configurar Base en MetaMask

- Network Name: Base
- RPC URL: https://mainnet.base.org
- Chain ID: 8453
- Currency Symbol: ETH
- Block Explorer: https://base.blockscout.com

Ya tengo la red configurada y lista para usar.

### Wallets recomendadas para usar con Base

- Coinbase Wallet → Mejor integración nativa
- MetaMask → Muy flexible y popular
- Rainbow → Buena experiencia mobile
- Trust Wallet → Simple para principiantes

Cada una tiene sus ventajas según lo que quieras hacer.

### Cómo verificar un contrato en Blockscout (Base)

Después de desplegar es importante verificar el código fuente para que sea transparente.

Pasos básicos:
- Ir a Blockscout
- Buscar el contrato
- Subir el código fuente y metadata

### Tipos de transacciones comunes en Base

- Transferencias simples de ETH o tokens
- Interacción con contratos (swaps, mint, etc.)
- Despliegue de contratos
- Aprobaciones (approve)

Cada una tiene un costo de gas diferente.

### Diferencias clave Base vs Ethereum Mainnet

- Fees: Base es mucho más barato
- Velocidad: Confirmaciones más rápidas en Base
- Seguridad: Base hereda seguridad de Ethereum
- Uso: Base es mejor para experimentar y dApps diarias

- ### Introducción a los tokens ERC-20

Los ERC-20 son el estándar más común para crear tokens fungibles en Base (como stablecoins o tokens de proyectos).

Características principales:
- Transferibles
- Aprobaciones (approve)
- Balance consultable

- ### Oráculos en blockchain

Los oráculos traen datos del mundo real (precios, resultados deportivos, clima, etc.) a los contratos inteligentes.

En Base se pueden usar Chainlink y otros oráculos compatibles.

### Cómo funciona un bridge paso a paso

1. Depositas fondos en la red origen
2. El bridge bloquea o quema los fondos
3. Se emiten los fondos equivalentes en Base
4. Puedes retirar en la otra dirección

Siempre verificar el bridge oficial.

### Remix IDE como herramienta inicial

Remix es un editor online perfecto para empezar con Solidity:
- No requiere instalación
- Permite compilar y desplegar directamente
- Ideal para pruebas rápidas en Base

- ### Introducción a Chainlink

Chainlink es un oráculo descentralizado que proporciona datos confiables a los contratos inteligentes (precios, randomness, etc.).

Es muy usado en Base y otras L2s.

### Contrato Counter simple (primer experimento)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Counter {
    uint256 public count = 0;

    function increment() public {
        count += 1;
    }

    function decrement() public {
        count -= 1;
    }

    function reset() public {
        count = 0;
    }
}
