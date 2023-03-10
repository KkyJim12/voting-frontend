<template>
  <div class="h-screen bg-blue-500 p-10">
    <div class="grid grid-cols-12 h-full gap-x-10">
      <div class="col-span-3">
        <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
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
              class="rounded bg-red-500 hover:bg-red-600 px-4 py-2 text-white shadow-md flex items-center space-x-4 w-full justify-center"
            >
              <img
                class="w-6 h-6"
                src="~/assets/image/metamask.png"
                alt="metamask"
              />
              <span> Connect to metamask</span>
            </button>
          </form>
        </div>
      </div>
      <div class="col-span-9">
        <div class="bg-white h-full bg-opacity-70 p-8 shadow-md">
          <h1 class="text-2xl font-semibold">Candidate List</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>
