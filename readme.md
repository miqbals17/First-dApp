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


## Membuat Halaman Web Sederhana
1. Buat folder baru dengan menggunakan terminal, gunakan `mkdir <nama direktori>`
2. Buat file dengan nama `index.html`
3. Buka index.html
4. Buat HTML *Boilerplate*
    ```
    <!DOCTYPE html>
    <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta http-equiv="X-UA-Compatible" content="IE=edge">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>My First dApp</title>
      </head>

      <body></body>
    </html>
    ```
    
5. Di dalam tag body tambahkan keterangan, label, dan input
    ```
    <body>
      <div>
        <h1>This is my dApp!</h1>
        <p>Here we can set or get the mood:</p>
        <label for="mood">Input Mood:</label> <br />
        <input type="text" id="mood" />
      </div>
    </body>
    ```
    
6. Di dalam tag div tambahkan buttons
    ```
    <button onclick="getMood()">Get Mood</button>
    <button onclick="setMood()">Set Mood</button>
    ```
    
**OPTIONAL** Tambahkan styling pada tag head untuk tampilan yang lebih terstruktur
    
    <style>
      body {
        text-align: center;
        font-family: Arial, Helvetica, sans-serif;
      }

      div {
        width: 20%;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
      }

      button {
        width: 100%;
        margin: 10px 0px 5px 0px;
      }
    </style>
    
8. Buka halaman web pada root folder dari `index.html` dengan menggunakan
    ```
    lite-server
    ```
    
9. Pergi ke [http://127.0.0.1:3000/](http://127.0.0.1:3000/) untuk melihat halaman web yang telah dijalankan
