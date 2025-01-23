<script>
  import { onMount } from "svelte";
  import yoga from "../images/yoga_11063629.png";

  let durationInSeconds = "60";
  let meditationInputElement;
  let meditationPrompt = "";
  let musicPrompt = "";
  let email = "";
  let isAudioHidden = false;
  let isLoading = false;
  let audioSrc;

  onMount(() => {
    meditationInputElement.focus();
  });

  function handleClick() {
    console.log("clicked");
    isAudioHidden = false;
    isLoading = true;
    fetch("https://g5m4ju.buildship.run/meditate-gpt-68bf08b63895", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        duration: durationInSeconds,
        meditationPrompt: meditationPrompt,
        musicPrompt: musicPrompt,
        email: email,
      }),
    })
      .then((response) => response.text())
      .then((data) => {
        console.log("Success:", data);
        audioSrc = data;
        isAudioHidden = true;
      })
      .finally(() => {
        console.log("finally");
        isAudioHidden = true;
        isLoading = false;
      });
  }
</script>

<header>
  <div class="flex-row pad-16">
    <h1>Meditate GPT</h1>
    <div>
      <img src={yoga} alt="yoga" width="60" />
    </div>
  </div>
  <div>Powered by BuildShip</div>
</header>
<main>
  <div>
    <div>Description</div>
    <input
      type="text"
      bind:value={meditationPrompt}
      placeholder="I'm feeling nostalgic, help me focus on the present and future"
      bind:this={meditationInputElement}
    />
  </div>
  <div>
    <strong>Describe your meditation session</strong>
  </div>
  <div>
    <div>Meditation duration</div>
    <input
      type="radio"
      id="60"
      value="60"
      name="duration"
      bind:group={durationInSeconds}
      checked={durationInSeconds === "60"}
    />
    <label for="60">1 min</label>
    <input
      type="radio"
      id="180"
      value="180"
      name="duration"
      bind:group={durationInSeconds}
    />
    <label for="180">3 mins</label>
    <input
      type="radio"
      id="600"
      value="600"
      name="duration"
      bind:group={durationInSeconds}
    />
    <label for="600">10 mins</label>
  </div>
  <div>
    <div>Email</div>
    <div>
      Enter your email to receive the meditation audio via email once it is
      generated
    </div>
    <input type="email" bind:value={email} placeholder="you@example.com" />
  </div>
  <br />
  <button onclick={handleClick}>Start meditation</button>
  {#if isAudioHidden}
    <div>
      <p>
        Generate a meditation session complete with a relaxing background,
        featuring gentle music, and guided breathing exercise to help you
        achieve a state of deep relaxation and mindfulness.
      </p>
      <audio controls>
        <source src={audioSrc} type="audio/mpeg" />
        Your browser does not support the audio element.
      </audio>
    </div>
  {/if}
  {#if isLoading}
    <div>
      <p>
        Crafting a meditation track to gently guide your mind into a state of
        tranquility. Generally takes about a minute.
      </p>
    </div>
  {/if}
</main>

<style>
  :root {
    --color-bg: #000;
    --color-text: #fff;
    --color-text-secondary: #aaa;
    --color-link: #fafafa;
  }

  .flex-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }

  .pad-16 {
    padding: 16px 0 0;
  }

  h1 {
    margin-bottom: 0.2rem;
  }

  button {
    padding: 0.6rem 2rem;
    font-weight: normal;
  }
</style>
