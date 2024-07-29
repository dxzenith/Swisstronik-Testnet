## DEPLOY A SIMPLE CONTRACT USING HARDHAT

- Open Github Codespace/ Gitpod / Linux based Terminal (Ubuntu, WSL), Here is the guide how to install Ubuntu : [Click Here](https://github.com/dxzenith/nodes-faq)
```bash
cd $HOME
```
- Create a folder
```bash
mkdir Swiss
```
```bash
cd Swiss
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
- Press `Enter` then again `Enter` then `y` and again `y`

```bash
rm hardhat.config.js
```
```bash
nano hardhat.config.js
```
- Now paste these things in `hardhat.config.js` file, also make sure to input your private key
```bash
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  solidity: "0.8.19",
  networks: {
    swisstronik: {
      url: "https://json-rpc.testnet.swisstronik.com/",
      accounts: ["0xd5..."], //Your private key starting with "0x"
    },
  },
};
```
- Use `W`, `A`, `S`, `D` Key to move your cursor
- To save this file use `Ctrl+X` then `Y` and then press `Enter`
```bash
cd contracts
```
```bash
rm Lock.sol
```
```bash
nano Lock.sol
```
- Now paste these codes in the above file (`Lock.sol`)
```bash
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.19;

contract Swisstronik {
    string private message;

    constructor(string memory _message) payable{
        message = _message;
    }

    function setMessage(string memory _message) public {
        message = _message;
    }

    function getMessage() public view returns(string memory){
        return message;
    }
}
```
- To save this file use `Ctrl+X` then `Y` and then press `Enter`
- Now use the below command to compile ur contract file (`Lock.sol`)
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
- Paste these codes in `deploy.js` file
```bash
const hre = require("hardhat");

async function main() {

  const contract = await hre.ethers.deployContract("Swisstronik", ["Hello Swisstronik!!"]);

  await contract.waitForDeployment();

  console.log(`Swisstronik contract deployed to ${contract.target}`);
}
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
- To save this file use `Ctrl+X` then `Y` and then press `Enter`
```bash
cd
```
```bash
cd Swiss
```
```bash
npx hardhat run scripts/deploy.js --network swisstronik
```
- After successful deployment, you should receive the following message in your terminal: `Swisstronik contract deployed to 0x...`
- Copy this contract address, you need to submit it on Testnet page
```bash
nano hardhat.config.js
```
- Now delete your private key from here.
- Then to save this file use `Ctrl+X` then `Y` and then press `Enter`
- Now you need to upload these files on your github repository
- Follow [this guide](https://github.com/dxzenith/Swisstronik-Testnet/blob/main/Upload-To-Github.md) to upload these files in your github repository
- Then submit your contract address and github repo link [here](https://www.swisstronik.com/testnet2/dashboard)
