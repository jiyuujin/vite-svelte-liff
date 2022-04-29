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
  <h1>Liff Demo</h1>
  {#await promise}
    <p>LIFF init...</p>
  {:then}
    <p>LIFF init succeeded.</p>
    <h4 class="error">{errorMessage}</h4>
    <h3>LIFF Info</h3>
    <ul>
      <li>
        <strong>LIFF Browser</strong>
        :<span>{liff.isInClient()}</span>
      </li>
      <li>
        <strong>Login Status</strong>
        :<span>{liff.isLoggedIn()}</span>
      </li>
      <li>
        <strong>Language</strong>
        :<span>{liff.getLanguage()}</span>
      </li>
      <li>
        <strong>OS</strong>
        :<span>{liff.getOS()}</span>
      </li>
      <li>
        <strong>LIFF Ver</strong>
        :<span>{liff.getVersion()}</span>
      </li>
      <li>
        <strong>LINE Ver</strong>
        :<span>{liff.getLineVersion()}</span>
      </li>
    </ul>
    <hr />
    <h3>LIFF QR Code</h3>
    <img src="./liff-qr.png" alt="" />
  {:catch e}
    <p>LIFF init failed.</p>
    <p><code>{`${e}`}</code></p>
  {/await}
</main>

<style>
  main {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .error {
    color: red;
  }
  ul {
    list-style-type: none;
    text-align: left;
    margin: 0;
    padding: 0;
  }
  li {
    padding: 5px;
    border: 1px solid #dddddd;
  }
  li span {
    margin: 5px;
  }
</style>
