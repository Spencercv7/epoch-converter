<script lang="ts">
	import { fly } from 'svelte/transition';
    import Socials from './Socials.svelte';

    let inputEpochDate: number;
    $: inputEpochDate = 0;

    let inputHumanDate: string;
    $: inputHumanDate = "";

    let showMenu: boolean;
    $: showMenu = false;

    let currentDate: number;
    $: currentDate = Date.now();

    const formats: string[] = ["millisecond", "second", "minute", "day"];

	let currentEpochDateFormat: string;
    $: currentEpochDateFormat = formats[0];

	let currentlySelectedInputField: string;
	$: currentlySelectedInputField = null;
    
    let autoUpdate: NodeJS.Timeout = SetDefaultState();

    function SetDefaultState(): NodeJS.Timeout {
        return setInterval(() => {
            currentDate = Date.now()
			inputEpochDate = getEpochDate(currentDate);
			inputHumanDate = getHumanDate(currentDate);
        }, 1000)
    }

    const handleChangeEpochFormat = (format: string): void => {
		currentEpochDateFormat = format;
        inputEpochDate = getEpochFromMillisecond(currentDate, format);
    }

	const getEpochDate = (currentDate: number): number => {
		return getEpochFromMillisecond(currentDate, currentEpochDateFormat);
	}

	const getHumanDate = (currentDate: number): string => {
		return new Date(currentDate).toDateString();
	}

	const handleClearDefaultState = (): void => {
		clearInterval(autoUpdate);
	}

	function getMillisecondFromEpoch(epoch: number, currentFormat: string): number {
		switch (currentFormat) {
			case formats[1]: // s
				return epoch * 1000;
			case formats[2]: // m
				return epoch * 60000;
			case formats[3]: // d
				return epoch * 86400000
			default:         // ms
				return epoch;
		}
	}
	
	function getEpochFromMillisecond(epoch: number, currentFormat: string): number {
		let result: number;
		switch (currentFormat) {
			case formats[1]: // s
				result = epoch / 1000;
				break;
			case formats[2]: // m
				result = epoch / 60000;
				break;
			case formats[3]: // d
				result = epoch / 86400000
				break;
			default:         // ms
				result = epoch;
				break;
		}
		return Math.floor(result);
	}

	$: getProcessedHumanDate = (): string => {
		return new Date(Date.parse(inputHumanDate)).toDateString();
	}

	// Instatiated as false...
	const handleMenuClick = (): void => {
        showMenu = !showMenu;
    }

</script>

<main>
    <Socials />
    <h1><span>Epoch Converter</span></h1>
    <form
    >
        <div class="input_wrapper">
            <label for="epoch_date">
                Unix Timestamp: 
            </label>
            <input 
                type="number" 
                id="epoch_date" 
                bind:value={inputEpochDate} 
				on:click={() => handleClearDefaultState()}
            />
            <div id="dropdown_menu_wrapper"
                on:click={() => handleMenuClick()}
                >
                    <button 
                        id="dropdown_button"
                        on:click|preventDefault
                    >
                        {currentEpochDateFormat}
                    </button>
                    {#if showMenu}
                        <ul in:fly="{{y: -30, duration: 200}}" out:fly="{{y: 30, duration: 200}}" id="dropdown_menu">
                            {#each formats as format}
                                <li 
                                    on:click={() => handleChangeEpochFormat(format)} 
                                    style={currentEpochDateFormat == format ? "color: var(--color-primary)" : ""};
                                >
                                    {format}
                                </li>
                            {/each}
                        </ul>
                    {/if}
                </div>
        </div>
        <div class="input_wrapper">
            <label for="human_date">Standard Date</label>
            <textarea
                rows={2}
                id="human_date" 
                bind:value={inputHumanDate}
				on:click={() => handleClearDefaultState()}
            />
            {#if inputHumanDate}
                <p><span>Input Parsed as: </span>{getProcessedHumanDate()}</p>
            {/if}
        </div>
    </form>
</main>


<style>
    
	main {
        height: 100%;
        min-height: 100vh;
        display: grid;
        justify-items: center;
        align-items: center;

        display: grid;
        grid-template-rows: auto 1fr;

        background-color: var(--color-grey);
	}

    h1 {
        padding: 2rem;
        color: var(--color-primary);
        text-decoration: underline;
        font-size: var(--font-size-h1);
        justify-self: left;
    }

    h1 span {
        color: var(--color-white);
    }

    form {
        position: relative;
        display: grid;
        grid-template-rows: repeat(2, 1fr);
        grid-gap: 2rem;
        width: 80%;
        align-items: center;
    }

    .input_wrapper {
        position: relative;
        display: grid;
        border-left: var(--border);
        border-bottom: .3rem solid transparent;
        border-top: .3rem solid transparent;
        padding-left: 1rem;
    }

    #dropdown_menu {
        background-color: var(--color-grey);
        list-style: none;
        font-size: var(--font-size-small);
        border-top: var(--border);
        border-right: var(--border);
        border-bottom: var(--border);
        display: flex;
    }

    li {
        padding: .5rem .5rem;
        transition: all .1s;
        cursor: pointer;
        color: var(--color-white);
    }

    li:hover {
        transform: translateY(-.2rem);
    }

    #dropdown_menu_wrapper {
        display: flex;
        width: max-content;
    }

    label {
        color: var(--color-primary);
        font-size: var(--font-size-small);
        position: relative;
        width: 100%;
        display: flex;
        grid-gap: .5rem;
    }

    input, textarea {
        font-size: var(--font-size-med);
        background-color: transparent;
        border: none;
        color: var(--color-white);
        width: 100%;
    }

    textarea {
        resize: none;
    }

    input:focus-visible, textarea:focus-visible{
        outline: none;
    }

    p {
        color: var(--color-white);
        font-size: var(--font-size-small);
    }

    p span {
        color: var(--color-primary);
    }

    div span {
        position: relative;
        width: max-content;
        display: flex;
        grid-gap: .2rem;
        align-items: center;
    }

    button {
        min-width: 8em;
        position: relative;
        appearance: none;
        color: var(--color-white);
        font-size: var(--font-size-small);
        border: var(--border);
        background-color: transparent;
        padding: .5rem .5rem;
        transition: all .2s;
        cursor: pointer;
        z-index: 100;
    }

    @media only screen and (max-height: 650px) {
        form {
            grid-gap: 1rem;
        }
    }

</style>