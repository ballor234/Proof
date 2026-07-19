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

### Verificación de firma (EIP-712)

```solidity
function verifySignature(address signer, bytes32 hash, bytes memory signature) internal pure returns (bool) {
    // lógica de verificación
}

### Uso básico de Chainlink VRF

```solidity
import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";

contract RandomNumber is VRFConsumerBaseV2 {
    function requestRandomWords() external {
        // solicitud de número aleatorio
    }
}

### Uso de Chainlink Price Feed

```solidity
import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

contract PriceConsumer {
    AggregatorV3Interface internal priceFeed;

    constructor() {
        priceFeed = AggregatorV3Interface(0x...); // address en Base
    }

    function getPrice() public view returns (int256) {
        (,int256 price,,,) = priceFeed.latestRoundData();
        return price;
    }
}

### Lógica de limit buy/sell

```solidity
function buyWithLimit(uint256 maxPrice) public payable {
    require(getCurrentPrice() <= maxPrice, "Price too high");
    // execute buy
}
### Token con tax on transfer

```solidity
function _transfer(address from, address to, uint256 amount) internal override {
    uint256 tax = amount * 5 / 100;
    super._transfer(from, address(this), tax);
    super._transfer(from, to, amount - tax);
}

### Burn mechanism

```solidity
function burn(uint256 amount) public {
    _burn(msg.sender, amount);
}

function _burn(address account, uint256 amount) internal {
    totalSupply -= amount;
    balanceOf[account] -= amount;
    emit Transfer(account, address(0), amount);
}

### Mecanismo de reflection básico

```solidity
uint256 public reflectionRate = 100;

function _transfer(...) internal {
    uint256 reflection = amount * reflectionRate / 1000;
    // distribuir a holders
}

### Max wallet limit

```solidity
uint256 public maxWalletPercent = 2; // 2% del supply

function _transfer(...) internal override {
    require(balanceOf[to] + amount <= maxWalletSize, "Exceeds max wallet");
    super._transfer(...);
}

### Tax dinámico

```solidity
uint256 public currentTax = 10;

function setTax(uint256 newTax) public onlyOwner {
    currentTax = newTax;
}

function _transfer(...) {
    uint256 tax = amount * currentTax / 100;
    // apply tax
}

### Mecanismo de buyback

```solidity
function buyback() public {
    uint256 contractBalance = address(this).balance;
    // swap ETH por token y burn o distribute
}

### Charity / Donation wallet

```solidity
address public charityWallet;

function setCharityWallet(address _wallet) public onlyOwner {
    charityWallet = _wallet;
}

 // parte de la tax va a charityWallet

### Blacklist básico

```solidity
mapping(address => bool) public isBlacklisted;

function blacklist(address account) public onlyOwner {
    isBlacklisted[account] = true;
}

modifier notBlacklisted(address account) {
    require(!isBlacklisted[account], "Blacklisted");
    _;
}

### Snapshot de balances

```solidity
mapping(uint256 => mapping(address => uint256)) public balanceAtSnapshot;

function takeSnapshot() public {
    // guarda balances en un ID de snapshot
}

### Merkle Proof para whitelist

```solidity
import "@openzeppelin/contracts/utils/cryptography/MerkleProof.sol";

function mintWhitelist(bytes32[] calldata proof) public {
    require(MerkleProof.verify(proof, merkleRoot, keccak256(abi.encodePacked(msg.sender))), "Not whitelisted");
}

### Soporte de royalties ERC2981

```solidity
import "@openzeppelin/contracts/token/common/ERC2981.sol";

contract MyNFT is ERC721, ERC2981 {
    function royaltyInfo(uint256 tokenId, uint256 salePrice) public view override returns (address, uint256) {
        return (royaltyReceiver, (salePrice * 5) / 100); // 5%
    }
}

### Patrón Upgradeable (UUPS)

Permite actualizar la lógica del contrato sin perder el estado.

Usando OpenZeppelin upgrades.

### Cross-chain con LayerZero o Axelar

```solidity
// Ejemplo básico con LayerZero
function sendCrossChain(...) public payable {
    // enviar mensaje a otra chain
}

### Governance básico (timelock + voting)

```solidity
contract SimpleGovernance {
    uint256 public proposalCount;
    mapping(uint256 => Proposal) public proposals;

    struct Proposal {
        string description;
        uint256 voteFor;
        uint256 voteAgainst;
        uint256 endTime;
    }
}

### Vote Escrowed Token (veToken)

```solidity
// Usuario lockea tokens por tiempo
// Recibe poder de voto proporcional al tiempo locked

### Flash Loan básico

```solidity
function flashLoan(uint256 amount, address target) public {
    uint256 balanceBefore = token.balanceOf(address(this));
    token.transfer(target, amount);
    // ejecutar lógica en target
    require(token.balanceOf(address(this)) >= balanceBefore + fee, "Flash loan not repaid");
}

### Lending básico

```solidity
mapping(address => uint256) public supplied;

function supply(uint256 amount) public {
    token.transferFrom(msg.sender, address(this), amount);
    supplied[msg.sender] += amount;
}

function borrow(uint256 amount) public {
    // lógica con collateral
}

### Opciones básicas (call/put)

Contrato que permite comprar el derecho a comprar/vender un asset a un precio fijo en el futuro.

### Prediction Market básico

```solidity
mapping(uint256 => uint256) public yesVotes;
mapping(uint256 => uint256) public noVotes;

function vote(uint256 marketId, bool prediction) public {
    // lógica de voto con tokens
}

### Insurance pool básico

```solidity
mapping(address => uint256) public coverage;

function buyCoverage(uint256 amount) public payable {
    // pagar premium
    coverage[msg.sender] += amount;
}

### Yield Optimizer básico

```solidity
contract YieldOptimizer {
    function deposit(uint256 amount) public {
        // deposita en el mejor vault actual
    }

    function harvest() public {
        // reclama rewards y reinvierte
    }
}

### Perpetual futures básico

Contrato que permite posiciones long/short con leverage sin fecha de expiración.

### Social features on-chain

```solidity
mapping(address => string) public profileName;
mapping(address => string) public status;

function updateProfile(string memory name, string memory status) public {
    profileName[msg.sender] = name;
    // ...
}

### Referral rewards

```solidity
mapping(address => uint256) public referralRewards;

function claimReferralReward() public {
    uint256 reward = referralRewards[msg.sender];
    referralRewards[msg.sender] = 0;
    token.transfer(msg.sender, reward);
}
