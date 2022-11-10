# Membuat dApps Pertama Kita

Berikut adalah step-by-step cara untuk membuat front end, deploy smart contract dengan bahasa Solidity, dan mengkoneksikan semuanya. Kita akan mengunakan [Metamask](https://metamask.io/), [Remix IDE](https://remix-project.org/), dan [Ethers.js](https://docs.ethers.io/v5/)

Pada tahap akhir dari tutorial ini, kita dapat membuat sebuah halaman front end HTML sederhana dengan buttons yang dapat berinteraksi dengan fungsi smart contract. Tutorial ini akan terbagi dalam 3 tahapan
  - Membuat halaman HTML sederhana
  - Membuat smart contract Solidity sederhana
  - Mengkoneksikan halaman web dengan smart contract menggunakan Ethers.js

## Perisapan
1. **Download and Install** [MetaMask](https://metamask.io/)
2. **Request ether dari faucet Goerli Testnet ke akun Metamask wallet**
    - [Request faucet](https://faucets.chain.link/)
3. **Install http server. Dapat menggunakan apa saja, disini kita akan menggunakan `lite-server`**
    - Install [Node.js](https://nodejs.org/en/)
    - Install lite-server dengan menggunakan npm pada terminal
      `npm install -g lite-server`
