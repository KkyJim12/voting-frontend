<template>
    <div class="h-screen bg-blue-500 p-10">
        <div class="grid grid-cols-12 h-full gap-x-10">
            <div class="col-span-3">
                <div class="bg-white h-full bg-opacity-70 p-8 shadow-md flex flex-col space-y-10">
                    <div class="flex flex-col space-y-2">
                        <h1 class="text-2xl font-semibold mb-8">Account Detail</h1>
                        <button
                            v-if="!isConnectWallet"
                            @click="connectMetamask()"
                            type="button"
                            class="rounded bg-red-500 hover:bg-red-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
                        >
                            <img class="w-6 h-6" src="~/assets/image/metamask.png" alt="metamask" />
                            <span> Connect to metamask</span>
                        </button>
                        <button
                            v-else-if="!isCorrectNetwork"
                            @click="switchNetwork()"
                            type="button"
                            class="rounded bg-blue-500 hover:bg-blue-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
                        >
                            <span>Switch Network</span>
                        </button>
                        <div v-else class="flex flex-col space-y-2">
                            <p class="text-xs">Address: {{ account.address }}</p>
                            <p class="text-xs">Balance: {{ web3.utils.fromWei(account.balance, 'ether') }} eth</p>
                            <p class="text-xs">Network: Goerli</p>
                        </div>
                    </div>
                    <div class="flex flex-col space-y-2">
                        <h1 class="text-2xl font-semibold mb-8">Add candidate</h1>
                        <form
                            @submit.prevent="addCandidate()"
                            v-if="account.address === managerAddress"
                            class="flex flex-col space-y-4 items-center"
                        >
                            <div class="flex flex-col space-y-2 w-full">
                                <label class="text-sm">Full Name</label>
                                <input
                                    v-model="form.fullName"
                                    class="rounded px-4 py-2 focus:outline-blue-500"
                                    type="text"
                                    placeholder="Type candidate full name"
                                />
                            </div>
                            <div class="flex flex-col space-y-2 w-full">
                                <label class="text-sm">Age</label>
                                <input
                                    v-model="form.age"
                                    class="rounded px-4 py-2 focus:outline-blue-500"
                                    type="number"
                                    placeholder="Type candidate age"
                                />
                            </div>
                            <div class="flex flex-col space-y-2 w-full">
                                <label class="text-sm">Organization</label>
                                <input
                                    v-model="form.organization"
                                    class="rounded px-4 py-2 focus:outline-blue-500"
                                    type="text"
                                    placeholder="Type candidate organization  "
                                />
                            </div>
                            <button
                                type="submit"
                                class="rounded bg-green-500 hover:bg-green-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
                            >
                                <span>Add Candidate</span>
                            </button>
                        </form>
                        <div v-else class="">You are not manager.</div>
                    </div>
                </div>
            </div>
            <div class="col-span-9">
                <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
                    <h1 class="text-2xl font-semibold mb-8">Candidate List</h1>
                    <div class="grid grid-cols-4 gap-4">
                        <div v-for="candidate in candidates" :key="index" class="col-span-1">
                            <div class="rounded bg-sky-500 p-4 transform hover:-translate-y-5 duration-300 shadow-md">
                                <h2 class="font-semibold text-white text-8xl text-center mb-4">2</h2>
                                <h2 class="font-semibold text-white">{{ candidate.fullName }}</h2>
                                <p class="text-white">Age: {{ candidate.age }}</p>
                                <p class="text-white">{{ candidate.organization }}</p>
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
import { abi } from '~/static/abi.json';
const Web3 = require('web3');
export default {
    data() {
        return {
            accounts: [],
            web3: '',
            isCorrectNetwork: false,
            isConnectWallet: false,
            account: { address: '', balance: 0, network: '' },
            contractAddress: '0x7c5fB40E6E9Dcea2CabcC30b37965BF39D987BF2',
            managerAddress: null,
            candidates: [],
            form: {
                fullName: '',
                age: '',
                organization: '',
            },
        };
    },
    async fetch() {
        await this.getManagerAddress();
        await this.viewAllCandidates();
    },
    methods: {
        async getManagerAddress() {
            const contract = await new this.web3.eth.Contract(abi, this.contractAddress);
            this.managerAddress = await contract.methods.viewManagerAddress().call();
        },
        async connectMetamask() {
            if (window.ethereum) {
                this.accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
                this.web3 = new Web3(window.ethereum);
                this.account = {
                    address: await this.web3.utils.toChecksumAddress(this.accounts[0]),
                    balance: await this.web3.eth.getBalance(this.accounts[0]),
                    network: await this.web3.eth.net.getId(),
                };

                if (this.account) {
                    this.isConnectWallet = true;
                }

                if (this.account.network === 5) {
                    this.isCorrectNetwork = true;
                }

                await this.getManagerAddress();
            } else {
                alert('You must install metamask first.');
            }
        },
        async switchNetwork() {
            await window.ethereum.request({
                method: 'wallet_switchEthereumChain',
                params: [{ chainId: Web3.utils.toHex(5) }],
            });

            this.account = {
                address: await this.web3.utils.toChecksumAddress(this.accounts[0]),
                balance: await this.web3.eth.getBalance(this.accounts[0]),
                network: await this.web3.eth.net.getId(),
            };

            this.isCorrectNetwork = true;
        },
        async addCandidate() {
            const contract = await new this.web3.eth.Contract(abi, this.contractAddress);
            const response = await contract.methods
                .addCandidate(this.form.fullName, this.form.age, this.form.organization)
                .call();
            console.log(response);
        },

        async viewAllCandidates() {
            const contract = await new this.web3.eth.Contract(abi, this.contractAddress);
            this.candidates = await contract.methods.viewAllCandidates().call();
        },
    },
};
</script>
