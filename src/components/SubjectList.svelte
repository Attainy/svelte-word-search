<script>
    export let gameSetBox;
    export let id;

</script>

<a href="/#/game/{id}" class="subject-list">
    <h3>{gameSetBox.subject}</h3>
    <h4>{gameSetBox.title}</h4>
    <p>{gameSetBox.description}</p>
</a>

<style>
    .subject-list {
        display: flex;
        flex-direction: column;
        gap: 1rem;
        border: 1px solid lightgray;
        padding: 1rem;
        cursor: pointer;
        width: 32%;
        text-decoration: none;
        color: black;
    }

    h3 {
        font-size: 1.5rem;
        font-weight: bold;
        background-color: var(--btnColor);
        padding: 0.5rem;
        text-align: center;
    }
    
    h4 {
        font-size: 1.2rem;
        color: red;
    }

</style>