<template>
    <div class="flex flex-col space-y-2">
        <h1 class="text-2xl font-semibold mb-4">Account Detail</h1>
        <button
            v-if="isConnect === false"
            @click="connectMetamask()"
            type="button"
            class="rounded bg-red-500 hover:bg-red-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
        >
            <img class="w-6 h-6" src="~/assets/image/metamask.png" alt="metamask" />
            <span> Connect to metamask</span>
        </button>
        <button
            v-else-if="isCorrectNetwork === false"
            @click="switchNetwork()"
            type="button"
            class="rounded bg-blue-500 hover:bg-blue-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
        >
            <span>Click to switch to Goerli network</span>
        </button>
        <div v-else class="flex flex-col space-y-2">
            <p class="text-xs">Address: {{ account.address }}</p>
            <p class="text-xs">Balance: {{ account.balance }}</p>
            <p class="text-xs">Network: {{ account.network }}</p>
        </div>
    </div>
</template>

<script>
export default {};
</script>
