<script>
  import { onMount } from "svelte";
  import logo from "../images/logo.png";
  import ahoumLogo from "../images/ahoum-logo.png";

  let durationInSeconds = "180";
  let meditationInputElement;
  let meditationPrompt = "";
  let musicPrompt = "";
  let email = "";
  let phone = "";
  let isAudioVisible = false;
  let isLoading = false;
  let isLoadingComplete = false;
  let audioSrc =
    "https://storage.googleapis.com/buildship-vlui9b-asia-southeast1/e036673b-2393-4646-bda3-e0fdd12702b5-meditation.mp3";
  let color = "";
  let colordescription = "";

  onMount(() => {
    meditationInputElement.focus();
  });

  function handleClick() {
    console.log("clicked");
    isAudioVisible = true;
    isLoading = true;
    fetch("https://vlui9b.buildship.run/meditate-gpt-b5517a9775bc", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        duration: Number(durationInSeconds),
        meditationPrompt: meditationPrompt,
        musicPrompt: musicPrompt,
        email: email,
        phone: phone,
      }),
    })
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        return response.json();
      })
      .then((data) => {
        color = data?.color;
        colordescription = data?.hexcode;
        console.log("Success:", data);
        isLoadingComplete = true;
      })
      .finally(() => {
        console.log("finally");
        isAudioVisible = true;
        isLoading = false;
        isLoadingComplete = true;
      });
  }
</script>

<header>
  <div class="flex-row pad-30-top">
    <h1>SPIRAL OF LIFE</h1>
    <div>
      <img src={logo} alt="meditate gpt logo" width="80" />
    </div>
  </div>
  <div class="flex-row">
    <div>Powered by &nbsp;</div>
    <img src={ahoumLogo} alt="ahoum logo" width="80" />
  </div>
</header>
<main>
  <div>
    <label for="meditationPrompt"
      >What is a part of yourself that you are working on? What are the
      concerns, hindrances, gratitudes that you don't often talk about? (in 20
      words)</label
    >
    <textarea
      id="meditationPrompt"
      type="text"
      bind:value={meditationPrompt}
      placeholder=""
      bind:this={meditationInputElement}
      rows="3"
    ></textarea>
    <br />
  </div>
  <div>
    <label for="180">Duration</label>
    <input
      type="radio"
      id="180"
      value="180"
      name="duration"
      bind:group={durationInSeconds}
    />
    <label for="180">3 mins</label>
    <br /><br />
  </div>
  <div>
    <label for="email">Email</label>
    <input id="email" type="email" bind:value={email} />
    <br />
  </div>
  <div>
    <label for="phone">Phone</label>
    <input id="phone" type="phone" bind:value={phone} />
  </div>
  <br />
  <button onclick={handleClick} disabled={isLoading}>Personalise</button>
  {#if isAudioVisible}
    <div>
      <p>
        You shall recieve a personalised guidance audio on your email shortly.<br
        />
        Check your spam folder for AHOUM email.<br />
        Check the colour of the installation.
      </p>
      <audio controls>
        <source src={audioSrc} type="audio/mpeg" />
        Your browser does not support the audio element.
      </audio>
      <br />
      <br />
    </div>
  {/if}
  <br />
  {#if isLoadingComplete}
    <div>
      <div>{colordescription}</div>
      <br/>
      <div class="circle {color}">&nbsp;</div>
    </div>
  {/if}
</main>

<style>
  :root {
    --color-bg: #101010;
    --color-text: #fff;
    --color-text-secondary: #aaa;
    --color-link: #fafafa;
  }

  main {
    padding: 1rem 1rem 3rem;
  }

  .flex-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }

  h1 {
    margin: 0.2rem 0;
    font-size: 3rem;
    line-height: 3rem;
    padding-top: 30px;
  }

  .pad-30-top {
    padding-top: 30px;
  }

  textarea,
  input {
    background: #101010;
    color: #fff;
    margin-bottom: 4px;
  }

  textarea::placeholder,
  input::placeholder {
    color: #555;
  }

  textarea,
  textarea::placeholder {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
      Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
  }

  label {
    margin-bottom: 8px;
  }

  input[type="email"] {
    width: 100%;
  }

  #meditationPrompt:focus-visible,
  #email:focus-visible,
  #phone:focus-visible {
    border: solid 1px #fafafa;
    outline: solid 1px #fafafa;
  }

  button {
    padding: 0.6rem 2rem;
    font-weight: normal;
  }

  .color-box {
    width: 300px;
    height: 80px;
  }

  .circle {
    width: 100px;
    height: 100px;
    border-radius: 50%;
  }

  /* Chakra colors sourced from traditional meditation practices */
  .red {
    background-color: #ff0000; /* Root Chakra */
  }

  .orange {
    background-color: #ff7f00; /* Sacral Chakra */
  }

  .yellow {
    background-color: #ffff00; /* Solar Plexus Chakra */
  }

  .green {
    background-color: #00ff00; /* Heart Chakra */
  }

  .blue {
    background-color: #0000ff; /* Throat Chakra */
  }

  .indigo {
    background-color: #4b0082; /* Third Eye Chakra */
  }

  .violet {
    background-color: #8b00ff; /* Crown Chakra */
  }
</style>
