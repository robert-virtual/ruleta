<script lang="ts">
  interface ITopic {
    text: string;
    color: string;
    children: string[];
  }
  let person: ITopic = { color: "", text: "", children: [] };
  let people: ITopic[] = [];
  const colors = [];
  function random(max: number = 255) {
    return Math.round(Math.random() * max);
  }
  function personKeyup(
    e: KeyboardEvent & {
      currentTarget: EventTarget & HTMLInputElement;
    }
  ) {
    if (e.key == "Enter") {
      addPerson();
    }
  }
  function addPerson() {
    let idx = random(colors.length - 1);
    while (topics[idx].children.length != 0) {
      idx = random(colors.length - 1);
      console.log({ idx });
    }

    people = [...people, { ...person, color: colors[idx] }];
    topics[idx].children = [...topics[idx].children, person.text];
    person = { text: "", color: "", children: [] };
  }
  let topic: ITopic = { text: "", color: "", children: [] };
  let topics: ITopic[] = [];
  function topicKeyup(
    e: KeyboardEvent & {
      currentTarget: EventTarget & HTMLInputElement;
    }
  ) {
    if (e.key == "Enter") {
      addTopic();
    }
  }
  function addTopic() {
    const color = `rgb(${random()},${random()},${random()})`;
    topics = [...topics, { ...topic, color }];
    colors.push(color);
    topic = { text: "", color: "", children: [] };
  }
  function removeTopic(i: number) {
    topics = topics.filter((_, idx) => idx != i);
  }
</script>

<main>
  <div class="row justify-around">
    <div class="row">
      <input
        type="text"
        on:keyup={topicKeyup}
        placeholder="Add some topic"
        bind:value={topic.text}
      />
      <button on:click={addTopic} class="secundary">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 4.5v15m7.5-7.5h-15"
          />
        </svg>
      </button>
    </div>

    <div class="row">
      <input
        type="text"
        on:keyup={personKeyup}
        placeholder="Add people"
        bind:value={person.text}
      />
      <button on:click={addPerson} class="secundary">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 4.5v15m7.5-7.5h-15"
          />
        </svg>
      </button>
    </div>
  </div>
  <div class="row justify-around">
    <div class="col">
      {#each topics as topic, i (i.toString())}
        <div class="topic col gap-2" style={`background-color: ${topic.color};`}>
          <div class="row items-center ">
            <span class="grow-1">{topic.text}</span>
            <button on:click={() => removeTopic(i)}>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M6 18L18 6M6 6l12 12"
                />
              </svg>
            </button>
          </div>
          {#each topic.children as child}
            <p>{child}</p>
          {/each}
        </div>
      {/each}
    </div>

    {#if !(topics.length > 0 && people.length > 0)}
      <p class="topic row justify-center" />
    {/if}
    <div class="col">
      {#each people as person, i (i.toString())}
        <p
          class="topic row items-center gap-2"
          style={`background-color: ${person.color};`}
        >
          <span class="grow-1">{person.text}</span>

          <button
            on:click={() => (people = people.filter((_, idx) => idx != i))}
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </p>
      {/each}
    </div>
  </div>
</main>

<style>
  .row {
    display: flex;
    flex-direction: row;
  }
  .col {
    display: flex;
    flex-direction: column;
  }
  button.secundary {
    background-color: blueviolet;
    color: white;
    border: none;
    width: 5rem;
  }
  input {
    background: #333;
    font-size: 2rem;
    padding: 1rem;
    outline: none;
    border: none;
  }
  .topic {
    font-size: 2rem;
    padding: 1rem;
  }
  .justify-center {
    justify-content: center;
  }
  .justify-around {
    justify-content: space-around;
  }
  .items-center {
    align-items: center;
  }
  .gap-2 {
    gap: 2rem;
  }
  .grow-1 {
    flex-grow: 1;
  }
  button {
    background: none;
    color: white;
    border: none;
    width: 5rem;
  }
</style>

