<template>
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
                @click="test()"
                type="button"
                class="rounded bg-green-500 hover:bg-green-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
            >
                <span>Add Candidate !!</span>
            </button>
        </form>
    </div>
</template>

<script>
export default {};
</script>
