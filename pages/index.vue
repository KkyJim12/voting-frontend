<template>
    <div class="h-screen bg-blue-500 p-10">
        <div class="grid grid-cols-12 h-full gap-x-10">
            <div class="col-span-3">
                <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
                    <div class="flex flex-col space-y-12">
                        <AccountDetail />
                        <AddCandidate />
                    </div>
                </div>
            </div>
            <div class="col-span-9">
                <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
                    <CandidateCard />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
const Web3 = require('web3');
export default {
    data() {
        return {
            abi: [
                {
                    inputs: [
                        {
                            internalType: 'address',
                            name: '_managerAddress',
                            type: 'address',
                        },
                    ],
                    stateMutability: 'nonpayable',
                    type: 'constructor',
                },
                {
                    inputs: [
                        {
                            internalType: 'string',
                            name: '_fullName',
                            type: 'string',
                        },
                    ],
                    name: 'addCandidate',
                    outputs: [],
                    stateMutability: 'nonpayable',
                    type: 'function',
                },
                {
                    inputs: [],
                    name: 'viewCandidates',
                    outputs: [
                        {
                            components: [
                                {
                                    internalType: 'string',
                                    name: 'fullName',
                                    type: 'string',
                                },
                                {
                                    internalType: 'uint256',
                                    name: 'voteCount',
                                    type: 'uint256',
                                },
                            ],
                            internalType: 'struct Candidate[]',
                            name: '',
                            type: 'tuple[]',
                        },
                    ],
                    stateMutability: 'view',
                    type: 'function',
                },
                {
                    inputs: [
                        {
                            internalType: 'uint256',
                            name: '_candidateIndex',
                            type: 'uint256',
                        },
                    ],
                    name: 'vote',
                    outputs: [],
                    stateMutability: 'nonpayable',
                    type: 'function',
                },
            ],
            web3: '',
            isCorrectNetwork: false,
            isConnect: false,
            account: { address: '', balance: 0, network: '' },
            contractAddress: '0x77358fdf08d4ed3899746034b4803f5694a2f1e5',
        };
    },
    async mounted() {
        await this.initWeb3();
    },
    methods: {
        async connectMetamask() {
            if (window.ethereum) {
                const accounts = await window.ethereum.request({
                    method: 'eth_requestAccounts',
                });

                this.account = {
                    address: accounts[0],
                    balance: await this.web3.eth.getBalance(accounts[0]),
                    network: await this.web3.eth.net.getId(),
                };

                if (this.account) {
                    this.isConnect = true;
                }

                if (this.account.network === 5) {
                    this.isCorrectNetwork = true;
                }
            } else {
                alert('You must install metamask first.');
            }
        },
        async initWeb3() {
            const web3Provider = new Web3.providers.HttpProvider(
                'https://eth-goerli.g.alchemy.com/v2/X-coHiFGjrD_qvb2FIB8JhjuUu__mpNN'
            );
            this.web3 = new Web3(web3Provider);
        },
        async switchNetwork() {},
        async test() {
            const contract = await new this.web3.eth.Contract(this.abi, this.contractAddress);
            console.log(await contract.methods.viewCandidates().call());
        },
    },
};
</script>
