<script>
    import { onMount } from "svelte";
    import Header from "../components/Header.svelte";
    import SubjectList from "../components/SubjectList.svelte";
    import { getDatabase, ref, onValue } from "firebase/database";

    const db = getDatabase();
    const lineRef = ref(db, "line/");

    // 반응형 변수 선언. 자동으로 업데이트 됨
    $: allCreatedGame = [];

    // 화면이 렌더링될 때마다 onValue 호출될 수 있도록 하는 게 onMount
    onMount(() => {
        onValue(lineRef, (snapshot) => {
        const data = snapshot.val();
        allCreatedGame = Object.values(data).reverse(); // 최신 업로드글이 위로
        });
    });

    console.log('!!', allCreatedGame)

</script>


<Header />
<div class='main'>
    <div class="rep">
      <img src="https://thewordsearch.com/v4/img/word-search-puzzle.png" alt="game-img" />
      <div class='intro'>
        <h2>Word Search</h2>
        <p>We have the best collection of word search puzzles online, with new ones being added regularly.</p>
        <p>They are fun to play, but also educational, in fact, many teachers make use of them.</p>
        <p>Puzzles are 100% free to play and work on desktop pc, mac, mobile and tablet. Or you can go old school and print them to enjoy offline later.</p>
        <p>Plus, if you're feeling a little more adventurous, why not create your very own with our simple to use Word Search Maker, and then share them with your friends.</p>
        <p>To get started playing, just select a game from below. Best of luck.</p>
      </div>
    </div>

    <div class="game-select">
      {#if !allCreatedGame == undefined}
        {#each allCreatedGame as gameSetBox}
          <SubjectList gameSetBox={gameSetBox}/>
        {/each}
      {/if}
    </div>
    
</div>

<style>
  .game-select {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 2rem;
    gap: 1rem;
  }
</style>