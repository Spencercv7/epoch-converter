<script>
    import { fade } from 'svelte/transition';
</script>

<ul class="socials" transition:fade={{duration: 200}}>
    <li>
        <a href="https://www.linkedin.com/in/spencer-venable-31494a187">
            <img class="centered" src="/images/linkedin.svg" alt="LinkedIn">
        </a>
    </li>
    <li>
        <a href="https://github.com/Spencercv7/aim-trainer">
            <img class="centered" src="/images/github.svg" alt="GitHub">
        </a>
    </li>
</ul>

<style>
    .socials {
        position: absolute;
        top: 0;
        right: 0;

        display: flex;
        grid-gap: .5rem;

        padding: 2rem;
    }

    li {
        position: relative;

        border: var(--border);
        border-radius: 100%;

        list-style: none;
        
        padding: .5rem;

        transition: all .2s;

        cursor: pointer;
        height: 4rem;
        width: 4rem;
    }

    li:hover {
        transform: scale(1.1);
    }

    img {
        height: 50%;
        width: 50%;
    }

    @media only screen and (max-width: 600px) {
        li {
            height: 3rem;
            width: 3rem;
        }
    }

    @media only screen and (max-width: 400px) {
        .socials {
            flex-direction: column;
        }
    }

</style>