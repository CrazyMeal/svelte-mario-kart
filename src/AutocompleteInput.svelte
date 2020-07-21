<!-- Mostly inspired from https://github.com/elcobvg/svelte-autocomplete/issues/9 -->

<script>
    import { scale } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const regExpEscape = s => {
        return s.replace(/[-\\^$*+?.()|[\]{}]/g, "\\$&");
    };

    const KEY_CODE_ARROW_DOWN = 40;
    const KEY_CODE_ARROW_UP = 38;
    const KEY_CODE_ENTER = 13;
    const KEY_CODE_ESCAPE = 27;

    export let id = "";
    export let items = [];
    export let maxItems = 10;
    export let minChar = 1;
    export let name = "";
    export let results = [];
    export let value = {};

    let arrowCounter = 0;
    let input;
    let loading = false;
    let open = false;
    let search = "";
    let timer = null;
    let list;

    const clickAway = event => {
        timer = setTimeout(() => {
            open = false;
        }, 200);
    };

    const clickOn = event => clearTimeout(timer);

    const onChange = event => {
        const term = event.target.value;

        if (term.length >= Number(minChar)) {
            open = true;
            filterResults(term);
        } else {
            results = [];
        }
    };

    const filterResults = term => {
        results = items
            .filter(item => {
                if (typeof item !== "string") item = item.value || "";

                return item.toUpperCase().includes(term.toUpperCase());
            })
            .map((item, i) => {
                const text = typeof item !== "string" ? item.value : item;

                return {
                    key: item.key || i,
                    value: text,
                    label:
                        term.trim() === ""
                            ? text
                            : text.replace(
                                RegExp(regExpEscape(term.trim()), "i"),
                                "<span class='font-semibold'>$&</span>"
                            )
                };
            })
            .slice(0, maxItems);
        
        console.log(list);
        if (list) {
            console.log(results.length);
            const height = results.length >= maxItems ? maxItems - 1 : results.length;
            list.style.height = `${height * 2.25}rem`;
        }
    };

    const onKeyDown = event => {
        if (event.keyCode === KEY_CODE_ARROW_DOWN && arrowCounter < results.length - 1) {
            arrowCounter++;
        } else if (event.keyCode === KEY_CODE_ARROW_UP && arrowCounter > 0) {
            arrowCounter--;
        } else if (event.keyCode === KEY_CODE_ENTER) {
            event.preventDefault();

            if (arrowCounter === -1) arrowCounter = 0;

            close(arrowCounter);
        } else if (event.keyCode === KEY_CODE_ESCAPE) {
            event.preventDefault();
            close();
        }
    };

    const close = (index = -1) => {
        open = false;
        arrowCounter = -1;
        input.blur();

        if (index > -1) {
            value = results[index];
            search = results[index].value;
        } else {
            search = "";
        }
    };

    $: console.log(value);
</script>

<style>
    * {
        box-sizing: border-box;
        padding-left: 200px;
    }

    input {
        height: 2rem;
        font-size: 1rem;
        padding: 0.25rem 0.5rem;
    }

    .autocomplete {
        position: relative;
    }

    .hide-results {
        display: none;
    }

    .autocomplete-results {
        padding: 0;
        margin: 0;
        margin-left: -200px;
        border: 1px solid #dbdbdb;
        height: 6rem;
        overflow: auto;
        min-width: 50%;
        max-width: 70%;

        background-color: white;
        box-shadow: 2px 2px 24px rgba(0, 0, 0, 0.1);
        position: relative;
        z-index: 100;
    }

    .autocomplete-result {
        color: #7a7a7a;
        list-style: none;
        text-align: left;
        height: 2rem;
        padding: 0.25rem 0.5rem;
        cursor: pointer;
    }

    .autocomplete-result> :global(span) {
        background-color: none;
        color: #242424;
        font-weight: bold;
        
        
    }

    .autocomplete-result.is-active,
    .autocomplete-result:hover {
        background-color: #dbdbdb;
    }
</style>

<div class="autocomplete">
    <input on:blur={clickAway} on:focus={clickOn} on:keydown="{event => onKeyDown(event)}"
        on:input="{event => onChange(event)}" bind:value={search} bind:this={input} type="text"
        placeholder="Search track" {id} {name} />
    
    {#if loading}
    <slot>
        <p>Loading data...</p>
    </slot>
    {/if}

    {#if open}
          <div>
            {#if results.length === 0}
              <div>
                No matches
              </div>
            {:else}
                <div class="autocomplete-results" bind:this={list}>
                {#each results as result, i}
                    <div on:click="{()=>close(i)}" class="autocomplete-result{ i === arrowCounter ? ' is-active' : '' }">
                        {@html result.label}
                    </div>
                {/each}
                </div>
            {/if}
      </div>
    {/if}
  </div>