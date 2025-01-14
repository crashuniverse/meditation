<script>
  let durationInSeconds = 10;
  let meditationPrompt = "";
  let musicPrompt = "";
  let isAudioHidden = false;
  let isLoading = false;
  let audioSrc;

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
  <h1>rainforest</h1>
</header>
<main>
  <div>
    <div>Meditation prompt</div>
    <input
      type="text"
      bind:value={meditationPrompt}
      placeholder="I am stressed after a day of work and I want a relaxing evening meditation."
    />
  </div>
  <div>
    <div>Duration (in seconds)</div>
    <input
      type="radio"
      id="10"
      value="10"
      name="duration"
      bind:group={durationInSeconds}
      checked
    />
    <span>10</span>
    <input
      type="radio"
      id="60"
      value="60"
      name="duration"
      bind:group={durationInSeconds}
    />
    <span>60</span>
  </div>
  <div>
    <div>Music prompt</div>
    <input type="text" bind:value={musicPrompt} placeholder="calm" />
  </div>
  <br />
  <button onclick={handleClick}>Start meditation</button>
  {#if isAudioHidden}
    <div>
      <p>
        Immerse yourself in this serene audio journey, crafted to gently guide
        your mind into a state of deep meditation and tranquility.
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
  button {
    padding: 0.6rem 2rem;
  }
</style>
