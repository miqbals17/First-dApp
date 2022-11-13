# Membuat dApps Pertama Kita

Berikut adalah step-by-step cara untuk membuat front end, deploy smart contract dengan bahasa Solidity, dan mengkoneksikan semuanya. Kita akan mengunakan [Metamask](https://metamask.io/), [Remix IDE](https://remix-project.org/), dan [Ethers.js](https://docs.ethers.io/v5/)

Pada tahap akhir dari tutorial ini, kita dapat membuat sebuah halaman front end HTML sederhana dengan buttons yang dapat berinteraksi dengan fungsi smart contract. Tutorial ini akan terbagi dalam 3 tahapan
  - Membuat halaman HTML sederhana
  - Membuat smart contract Solidity sederhana
  - Mengkoneksikan halaman web dengan smart contract menggunakan Ethers.js

## Perisapan
1.  **Download and Install** [MetaMask](https://metamask.io/)
2.  **Request ether dari faucet Goerli Testnet ke akun Metamask wallet**
      - [Request faucet](https://faucets.chain.link/)

3.  **Install http server. Dapat menggunakan apa saja, disini kita akan menggunakan `lite-server`**
      - Install [Node.js](https://nodejs.org/en/)
      - Install lite-server dengan menggunakan npm pada terminal
        `npm install -g lite-server`

## Membuat Halaman Web Sederhana
1.  Buat folder baru dengan menggunakan terminal, gunakan `mkdir <nama direktori>`
2.  Buat file dengan nama `index.html`
3.  Buka index.html
4.  Buat HTML *Boilerplate*
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
    
5.  Di dalam tag body tambahkan keterangan, label, dan input
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
    
6.  Di dalam tag div tambahkan buttons
    ```
    <button onclick="getMood()">Get Mood</button>
    <button onclick="setMood()">Set Mood</button>
    ```
    **OPTIONAL** Tambahkan styling pada tag head untuk tampilan yang lebih terstruktur
    ```
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
    ```
    
8.  Buka halaman web pada root folder dari `index.html` dengan menggunakan
    ```
    lite-server
    ```
    
9.  Pergi ke [http://127.0.0.1:3000/](http://127.0.0.1:3000/) untuk melihat halaman web yang telah dijalankan. Tampilan halaman web akan seperti berikut ini
    ![image](https://user-images.githubusercontent.com/37977826/201115792-d108318d-0ce7-4d20-b043-37ab3efe2ee4.png)
    
## Membuat Smart Contract Sederhana
1.  Pada tutorial ini kita akan menggunakan [Remix IDE](https://remix-project.org/)
2.  Buat file baru bernama `mood.sol`
3.  Tulis contract sebagai berikut
    - Tuliskan lisensi dan spesifikasi dari versi solidity
      ```
      // SPDX-License-Identifier: MIT
      pragma solidity ^0.8.0;
      ```
    - Buat contract
      ```
      contract MoodDiary {
      
      }
      ```
    - Di dalam contract, buat variabel `mood`
      ```
      string mood;
      ```
    - Buat fungsi set dan get
      ```
      function setMood(string memory _mood) public {
        mood = _mood;
      }
      
      function getMood() public view returns(string memory) {
        return mood;
      }
      ```
      
4.  Deploy smart contract pada jaringan Testnet Goerli

## Menghubungkan Halaman Web dengan Smart Contract
Kembali ke text editor pada `index.html`, tambahkan kode berikut pada halaman html:
  1.  Import Ethers.js ke dalam file `index.html` di dalam tag script:
      ```
      <script
        src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js"
        type="application/javascript"
      ></script>

      <script>
        ////////////////////
        //TAMBAHKAN KODE DI SINI
        ////////////////////
      </script>
      ```
  2.  Di dalam tag script, import ABI smart contract dan tambahkan alamat contract dari blockchain provider
      ```
      const MoodContractAddress = "<contract address>";
      const MoodContractABI = <contract ABI>
      let MoodContract;
      let signer;
      ```
      ABI Smart contract didapatkan melalui Remix IDE setelah smart contract dicompile, nilai dari ABI smart contract kurang lebih akan seperti ini:
      ```
      [
        {
          "inputs": [],
          "name": "getMood",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "string",
              "name": "_mood",
              "type": "string"
            }
          ],
          "name": "setMood",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        }
      ]
      ```
  3.  Selanjutnya, kita akan mendefenisikan ethers provider. Dalam hal ini kita akan menggunakan Goerli:
      ```
      const provider = new ethers.providers.Web3Provider(window.ethereum, "goerli");
      ```
  4.  Lakukan request access ke wallet user dan menghubungkan *signer* ke akun metamask (kita akan menggunakan `[0]` sebagai default), dan mendefinisikan object contract dengan menggunakan contract address, ABI, dan *signer*
      ```
      provider.send("eth_requestAccounts", []).then(() => {
          provider.listAccounts().then((accounts) => {
              signer = provider.getSigner(accounts[0]);
              MoodContract = new ethers.Contract(
                  MoodContractAddress,
                  MoodContractABI,
                  signer
              );
          });
      });
      ```
  5.  Buat asynchronous functions untuk memanggil smart contract functions
      ```
      async function getMood() {
          const getMoodPromise = MoodContract.getMood();
          const Mood = await getMoodPromise;
          document.getElementById("output").innerHTML = "Output: <br/>" + Mood;
      }

      async function setMood() {
          const mood = document.getElementById("mood").value;
          const setMoodPromise = MoodContract.setMood(mood);
          await setMoodPromise;
      }
      ```
  6.  Hubungkan functions tersebut ke buttons pada file html
      ```
      <button onclick="getMood()">Get Mood</button>
      <button onclick="setMood()">Set Mood</button>
      ```
      
## Saatnya Melakukan Test
1.  Akses [http://127.0.0.1:3000/](http://127.0.0.1:3000/) pada browser anda

3.  Tes function dan approve transaksi yang dibutuhkan melalui Metamask. Waktu untuk eksekusi pada blockchain sekitar ~15 detik... silakan tunggu hingga selesai transaksi
4.  Cek informasi contract dan transaksi melalui [https://goerli.etherscan.io](https://goerli.etherscan.io)
5.  Buka console (`Ctrl + Shift + i`) pada browser dan lihat keajaiban ketika anda menekan tombol `Get Mood`
