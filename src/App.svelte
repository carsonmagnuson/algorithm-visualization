<script lang="ts">
  import svelteLogo from "./assets/svelte.svg";
  import Counter from "./lib/Counter.svelte";
  import { flip } from "svelte/animate";
  import { bubble } from "svelte/internal";
  const idea: string = "Sorting Algorithms";

  var arrBubble = popul([]);
  var arrMerge = popul([]);
  var count = 1;

  function popul(
    arr: { state: string; num: number }[]
  ): { state: string; num: number }[] {
    for (let i = 0; i < 40; i++) {
      let example = {
        state: "whitesmoke",
        num: Math.floor(Math.random() * 50 + 1),
      };
      arr.push(example);
    }
    return arr;
  }

  function sortBubble() {
    var i = 0;
    var j = 0;

    function task() {
      count++;
      console.log(count);
      if (j >= arrBubble.length - i - 1) {
        arrBubble[j].state = "#8fbc8f";
        j = 0;
        i++;
      }

      if (i < arrBubble.length) {
        if (arrBubble[j].num > arrBubble[j + 1].num) {
          var temp = arrBubble[j];
          arrBubble[j] = arrBubble[j + 1];
          arrBubble[j + 1] = temp;
          console.log(arrBubble);
        } else {
          arrBubble[j].state = "whitesmoke";
          arrBubble[j + 1].state = "crimson";
        }
        j++;
        setTimeout(task, 1);
      } else {
        arrBubble[0].state = "#8fbc8f";
      }
    }
    task();
  }

  async function sortMerge() {
    arrMerge = await rC(arrMerge);

    function rC(
      a: { state: string; num: number }[]
    ): Promise<{ state: string; num: number }[]> {
      return new Promise((resolve) => {
        setTimeout(async () => {
          if (a.length == 1) {
            resolve(a);
          }

          let aM: { state: string; num: number }[] = [];
          let lA = await rC(a.slice(0, Math.floor(a.length / 2))); //2
          let rA = await rC(a.slice(Math.floor(a.length / 2))); //4

          arrMerge = lA.concat(rA);
          let l = 0;
          let r = 0;

          while (l < lA.length || r < rA.length) {
            // console.log('l: ' + lA[l].num + ". r: " + rA[r].num);
            // console.log('lArray: ' + lA.length + ". rArray: " + rA.length);

            // console.log(lA[l].num);
            // console.log(rA[r].num);

            if (l == lA.length) {
              aM.push(rA[r]);
              r++;
            } else if (r == rA.length) {
              aM.push(lA[l]);
              l++;
            } else if (lA[l].num >= rA[r].num) {
              aM.push(rA[r]);
              r++;
            } else if (rA[r].num >= lA[l].num) {
              aM.push(lA[l]);
              l++;
            }
          }
          console.log("new aM: " + aM);
          resolve(aM);
        }, 30);
      });
    }
  }
</script>

<main>
  <h1>{idea}</h1>
  <div class="card" style="display: inline-block">
    <div>
      {#each arrBubble as num}
        <div
          style="height: {num.num}px;width: 10px;background-color: {num.state};display:
          inline-block;"
        />
      {/each}
    </div>
    <div class="card">
      <button on:click={sortBubble}>Bubble Sort</button>
    </div>
  </div>
  <div class="card" style="display: inline-block">
    <div>
      {#each arrMerge as num}
        <div
          style="height: {num.num}px;width: 10px;background-color: {num.state};display:
          inline-block;"
        />
      {/each}
    </div>
    <div class="card">
      <button on:click={sortMerge}>Merge Sort</button>
    </div>
  </div>

  <p>Powered by Svelte and Vite</p>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
  .square {
    width: 10px;
    background-color: whitesmoke;
    display: inline-block;
  }
</style>
