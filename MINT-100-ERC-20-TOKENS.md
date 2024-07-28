- Open Github Codespace/Gitpod/Linux based terminal
```bash
cd $HOME
```
```bash
mkdir Swiss2
```
```bash
cd Swiss2
```
```bash
sudo apt-get remove -y nodejs
```
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash && export NVM_DIR="/usr/local/share/nvm"; [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"; [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"; source ~/.bashrc; nvm install --lts; nvm use --lts
```
```bash
npm install --save-dev hardhat
```
```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox
```
```bash
npx hardhat
```
- Keep pressing. `Enter`
```bash
rm package.json
```
```bash
nano package.json
```
```bash
{
  "name": "hardhat-project",
  "devDependencies": {
    "@nomicfoundation/hardhat-toolbox": "^3.0.0",
    "hardhat": "^2.17.1"
  },
  "dependencies": {
    "@openzeppelin/contracts": "^4.9.3",
    "@swisstronik/utils": "^1.2.1"
  }
}
```
- Press `Ctrl + X` then `Y` and then press `Enter`
```bash
rm hardhat.config.js
```
```bash
nano hardhat.config.js
```
```bash
require("@nomicfoundation/hardhat-toolbox");

const PRIVATE_KEY = "PASTE UR PRIVATE KEY WITHOUT 0X";

module.exports = {
  defaultNetwork: "swisstronik",
  solidity: "0.8.19",
  networks: {
    swisstronik: {
      url: "https://json-rpc.testnet.swisstronik.com/",
      accounts: [`0x` + `${PRIVATE_KEY}`],
    },
  },
};
```
- Use `W`, `A`, `S`, `D` key to move the cursor
- Press `Ctrl + X` then `Y` and then press `Enter`
```bash
cd contracts
```
```bash
rm Lock.sol
```
```bash
nano Token.sol
```
```bash
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract TestToken is ERC20 {
    constructor()ERC20("ZunXBT","ZUN"){} 

    function mint100tokens() public {
        _mint(msg.sender,100*10**18);
    }

    function burn100tokens() public{
        _burn(msg.sender,100*10**18);
    }
    
}
```
- Press `Ctrl + X` then `Y` and then press `Enter`
```bash
npm install
```
```bash
npx hardhat compile
```
```bash
cd ..
```
```bash
mkdir scripts && cd scripts
```
```bash
nano deploy.js
```
```bash
const hre = require("hardhat");

async function main() {
  const contract = await hre.ethers.deployContract("TestToken");

  await contract.waitForDeployment();

  console.log(`deployed to ${contract.target}`);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
- Press `Ctrl + X` then `Y` and then press `Enter`
```bash
cd ..
```
```bash
npx hardhat run scripts/deploy.js --network swisstronik
```
- Copy the contract address and save it somewhere else, it will be required in the below tasks
```bash
cd scripts
```
```bash
nano mint.js
```
```bash
const hre = require("hardhat");
const { encryptDataField, decryptNodeResponse } = require("@swisstronik/utils");
const sendShieldedTransaction = async (signer, destination, data, value) => {
  const rpcLink = hre.network.config.url;
  const [encryptedData] = await encryptDataField(rpcLink, data);
  return await signer.sendTransaction({
    from: signer.address,
    to: destination,
    data: encryptedData,
    value,
  });
};

async function main() {
  const contractAddress = "ENTER YOUR CONTRACT ADDRESS HERE";
  const [signer] = await hre.ethers.getSigners();

  const contractFactory = await hre.ethers.getContractFactory("TestToken");
  const contract = contractFactory.attach(contractAddress);

  const functionName = "mint100tokens";
  const mint100TokensTx = await sendShieldedTransaction(
    signer,
    contractAddress,
    contract.interface.encodeFunctionData(functionName),
    0
  );

  await mint100TokensTx.wait();

  console.log("Transaction Receipt: ", mint100TokensTx.hash);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
- Use `W`, `A`, `S`, `D` key to move the cursor
- Press `Ctrl + X` then `Y` and then press `Enter`
```bash
nano transfer.js
```
```bash
const hre = require("hardhat");
const { encryptDataField, decryptNodeResponse } = require("@swisstronik/utils");
const sendShieldedTransaction = async (signer, destination, data, value) => {
  const rpcLink = hre.network.config.url;
  const [encryptedData] = await encryptDataField(rpcLink, data);
  return await signer.sendTransaction({
    from: signer.address,
    to: destination,
    data: encryptedData,
    value,
  });
};

async function main() {
  const replace_contractAddress = "PASTE YOUR CONTRACT ADDRESS HERE";
  const [signer] = await hre.ethers.getSigners();

  const replace_contractFactory = await hre.ethers.getContractFactory("TestToken");
  const contract = replace_contractFactory.attach(replace_contractAddress);

  const replace_functionName = "transfer";
  const replace_functionArgs = ["0x16af037878a6cAce2Ea29d39A3757aC2F6F7aac1", "1"];
  const transaction = await sendShieldedTransaction(signer, replace_contractAddress, contract.interface.encodeFunctionData(replace_functionName, replace_functionArgs), 0);

  await transaction.wait();
  console.log("Transaction Response: https://explorer-evm.testnet.swisstronik.com/tx/" + transaction.hash);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
- Use `W`, `A`, `S`, `D` key to move the cursor
- Press `Ctrl + X` then `Y` and then press `Enter`
```bash
cd ..
```
```bash
npx hardhat run scripts/mint.js --network swisstronik
```
```bash
npx hardhat run scripts/transfer.js --network swisstronik
```
- Copy the Tx URL u get after applying this command `npx hardhat run scripts/transfer.js --network swisstronik` and save the Tx hash somewhere as u need to provide it on testnet Website
---
- Remove your private key from `hardhat.config.js` file using below command
```bash
nano hardhat.config.js
```
- Use `W`, `A`, `S`, `D` key to move the cursor
- Press `Ctrl + X` then `Y` and then press `Enter`
- Now verify whether `hardhat.config.js` file still contain your Private key or not using below command
```bash
cat hardhat.config.js
```
---
- Now visit : [Click Here](https://github.com/dxzenith/Swisstronik-Testnet/blob/main/Upload-To-Github.md) to upload these codes in your github repository
