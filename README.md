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

**Mensaje:** `docs: script de despliegue básico con Hardhat`

**Texto para añadir:**

```markdown
### Script de despliegue básico (Hardhat)

```javascript
// deploy.js
const hre = require("hardhat");

async function main() {
  const Counter = await hre.ethers.getContractFactory("Counter");
  const counter = await Counter.deploy();

  await counter.deployed();
  console.log("Counter desplegado en:", counter.address);
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});

**Mensaje:** `docs: frontend básico para interactuar con Counter`

**Texto para añadir:**

```markdown
### Frontend básico para interactuar con el Counter

```html
<button onclick="increment()">Incrementar</button>
<p>Contador: <span id="value">0</span></p>

<script>
  // Código con ethers.js para llamar al contrato en Base
</script>

### Idea de mejora al Counter

Añadir:
- Eventos cuando se modifica el contador
- Ownership (solo owner puede resetear)
- Contador por usuario

Esto lo haría más útil como proyecto de práctica.

### Contrato SimpleStorage

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    string public data = "Hola Base";

    function setData(string memory _data) public {
        data = _data;
    }

    function getData() public view returns (string memory) {
        return data;
    }
}
### Contrato con ownership básico

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Owned {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        owner = newOwner;
    }
}
### Contrato con modifier personalizado

```solidity
modifier whenNotPaused() {
    require(!paused, "Contract is paused");
    _;
}

function pause() public onlyOwner {
    paused = true;
}

### Contrato de staking simple

```solidity
contract SimpleStake {
    mapping(address => uint256) public stakedAmount;
    uint256 public rewardRate = 100; // ejemplo

    function stake(uint256 amount) public {
        // lógica de stake
    }

    function claimReward() public {
        // lógica de rewards
    }
}

### Contrato con función pausable

```solidity
import "@openzeppelin/contracts/security/Pausable.sol";

contract MyContract is Pausable {
    function emergencyPause() public onlyOwner {
        _pause();
    }

    function normalFunction() public whenNotPaused {
        // lógica normal
    }
}

### Función de withdraw segura

```solidity
function withdraw() public {
    uint256 amount = balances[msg.sender];
    require(amount > 0, "No balance");
    balances[msg.sender] = 0;
    payable(msg.sender).transfer(amount);
}
### Función con timelock

```solidity
uint256 public unlockTime;

function lockFunds() public {
    unlockTime = block.timestamp + 7 days; // 7 días
}

function withdraw() public {
    require(block.timestamp >= unlockTime, "Still locked");
    // withdraw logic
}

### Uso de AccessControl (roles)

```solidity
import "@openzeppelin/contracts/access/AccessControl.sol";

contract RoleBased is AccessControl {
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");

    constructor() {
        _grantRole(DEFAULT_ADMIN_ROLE, msg.sender);
    }
}
### Fallback y receive functions

```solidity
receive() external payable {
    // se ejecuta al recibir ETH directo
}

fallback() external {
    // se ejecuta cuando no hay función que coincida
}

### Funciones view y pure

```solidity
function getBalance() public view returns (uint256) {
    return address(this).balance;
}

function add(uint256 a, uint256 b) public pure returns (uint256) {
    return a + b;
}

### Uso de Structs

```solidity
struct User {
    uint256 id;
    string name;
    uint256 balance;
}

mapping(address => User) public users;

### Herencia en Solidity

```solidity
contract BaseContract {
    uint256 public value;
}

contract ChildContract is BaseContract {
    function setValue(uint256 _value) public {
        value = _value;
    }
}

### Uso de Interfaces

```solidity
interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract MyContract {
    IERC20 public token;
}

### Fallback avanzado

```solidity
fallback() external payable {
    // lógica para manejar llamadas no reconocidas
    emit FallbackCalled(msg.sender, msg.value);
}

### Operaciones en batch (múltiples)

```solidity
function batchTransfer(address[] calldata recipients, uint256[] calldata amounts) public {
    require(recipients.length == amounts.length, "Length mismatch");
    for (uint i = 0; i < recipients.length; i++) {
        // transfer logic
    }
}
### Contrato Raffle simple

```solidity
contract SimpleRaffle {
    address[] public participants;
    address public winner;

    function enter() public payable {
        participants.push(msg.sender);
    }

    function pickWinner() public {
        // lógica simple de random (mejorar con Chainlink VRF en producción)
        uint256 index = block.timestamp % participants.length;
        winner = participants[index];
    }
}

### Sistema de referrals básico

```solidity
mapping(address => address) public referrer;

function register(address _referrer) public {
    referrer[msg.sender] = _referrer;
    // dar reward al referrer
}

### Función con cooldown

```solidity
mapping(address => uint256) public lastAction;

function action() public {
    require(block.timestamp > lastAction[msg.sender] + 1 days, "Cooldown active");
    lastAction[msg.sender] = block.timestamp;
    // acción
}
