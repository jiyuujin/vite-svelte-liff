<script lang="ts">
  import { onMount } from 'svelte'
  import liff from '@line/liff'

  let profile = {}
  let errorMessage = ''

  async function init() {
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID,
    })
  }

  const getLiffProfile = async () => {
    return await liff.getProfile()
  }

  let promise = init().then(() => {
    profile = getLiffProfile()
  })

  onMount(async () => {
    if (!liff.isInClient()) {
      liff.login()
      profile = getLiffProfile()
    }
  })
</script>

<main>
  <div class="min-h-full flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-md w-full space-y-8">
      <h1 class="mt-6 text-center text-4xl font-extrabold text-gray-900">Liff Demo</h1>
      {#await promise}
        <p class="text-center">LIFF init...</p>
      {:then}
        <p class="text-center">LIFF init succeeded.</p>
        <h4 class="text-amber-700">{errorMessage}</h4>
        <h3 class="mt-6 text-center text-3xl font-extrabold text-gray-900">LIFF Info</h3>
        <ul class="list-none text-center p-0 m-0">
          <li class="p-1 border">
            <strong>LIFF Browser</strong>
            :<span>{liff.isInClient()}</span>
          </li>
          <li class="p-1 border">
            <strong>Login Status</strong>
            :<span class="m-1">{liff.isLoggedIn()}</span>
          </li>
          <li class="p-1 border">
            <strong>Language</strong>
            :<span class="m-1">{liff.getLanguage()}</span>
          </li>
          <li class="p-1 border">
            <strong>OS</strong>
            :<span class="m-1">{liff.getOS()}</span>
          </li>
          <li class="p-1 border">
            <strong>LIFF Ver</strong>
            :<span class="m-1">{liff.getVersion()}</span>
          </li>
          <li class="p-1 border">
            <strong>LINE Ver</strong>
            :<span class="m-1">{liff.getLineVersion()}</span>
          </li>
        </ul>
      {:catch e}
        <p class="text-center">LIFF init failed.</p>
        <p><code>{`${e}`}</code></p>
      {/await}
    </div>
  </div>
</main>
