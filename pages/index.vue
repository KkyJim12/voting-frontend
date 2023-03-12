<template>
  <div class="h-screen bg-blue-500 p-10">
    <div class="grid grid-cols-12 h-full gap-x-10">
      <div class="col-span-3">
        <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
          <div class="flex flex-col space-y-12">
            <div class="flex flex-col space-y-2">
              <h1 class="text-2xl font-semibold mb-4">Account Detail</h1>
              <div v-if="isConnect" class="flex flex-col space-y-2">
                <p class="text-xs">Address: {{ account.address }}</p>
                <p class="text-xs">Balance: {{ account.balance }}</p>
                <p class="text-xs">Network: {{ account.network }}</p>
              </div>
              <button
                v-else
                @click="connectMetamask()"
                type="button"
                class="rounded bg-red-500 hover:bg-red-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
              >
                <img
                  class="w-6 h-6"
                  src="~/assets/image/metamask.png"
                  alt="metamask"
                />
                <span> Connect to metamask</span>
              </button>
            </div>
            <div class="flex flex-col space-y-2">
              <h1 class="text-2xl font-semibold mb-4">Add candidate</h1>
              <form class="flex flex-col space-y-4 items-center">
                <div class="flex flex-col space-y-2 w-full">
                  <label class="text-sm">Full Name</label>
                  <input
                    class="rounded px-4 py-2 focus:outline-blue-500"
                    type="text"
                    placeholder="Type candidate full name"
                  />
                </div>
                <div class="flex flex-col space-y-2 w-full">
                  <label class="text-sm">Age</label>
                  <input
                    class="rounded px-4 py-2 focus:outline-blue-500"
                    type="number"
                    placeholder="Type candidate age"
                  />
                </div>
                <div class="flex flex-col space-y-2 w-full">
                  <label class="text-sm">Organization</label>
                  <input
                    class="rounded px-4 py-2 focus:outline-blue-500"
                    type="text"
                    placeholder="Type candidate organization  "
                  />
                </div>
                <button
                  type="submit"
                  class="rounded bg-green-500 hover:bg-green-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
                >
                  <span>Add Candidate !!</span>
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
      <div class="col-span-9">
        <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
          <h1 class="text-2xl font-semibold mb-4">Candidate List</h1>
          <div class="grid grid-cols-4 gap-4">
            <div class="col-span-1">
              <div
                class="rounded bg-sky-500 p-4 transform hover:-translate-y-5 duration-300 shadow-md"
              >
                <h2 class="font-semibold text-white text-8xl text-center mb-4">
                  2
                </h2>
                <h2 class="font-semibold text-white">Piyakarn Nimmakulvirut</h2>
                <p class="text-white">Age: 26</p>
                <p class="text-white">Kuliber co.,ltd.</p>
                <button
                  class="rounded bg-green-500 hover:bg-green-600 shadow-md w-full text-white py-2 mt-4"
                  type="button"
                >
                  Vote !!
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const Web3 = require('web3')
export default {
  data() {
    return {
      web3: '',
      isConnect: false,
      account: { address: '', balance: 0, network: '' },
    }
  },
  async mounted() {
    await this.initWeb3()
    await this.setWeb3()
  },
  methods: {
    async setWeb3() {},
    async connectMetamask() {
      if (window.ethereum) {
        const accounts = await window.ethereum.request({
          method: 'eth_requestAccounts',
        })

        this.account = {
          address: accounts[0],
          balance: await this.web3.eth.getBalance(accounts[0]),
          network: await window.ethereum.networkVersion,
        }

        if (this.account) {
          this.isConnect = true
        }
      } else {
        alert('You must install metamask first.')
      }
    },
    async initWeb3() {
      const web3Provider = new Web3.providers.HttpProvider(
        'https://eth-goerli.g.alchemy.com/v2/X-coHiFGjrD_qvb2FIB8JhjuUu__mpNN'
      )
      this.web3 = new Web3(web3Provider)

      this.web3.eth.getBlockNumber().then((result) => {
        console.log('Latest Ethereum Block is ', result)
      })
    },
  },
}
</script>
