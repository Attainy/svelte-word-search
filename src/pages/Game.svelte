<script>
    import GridBox from "../components/GridBox.svelte"
    import Header from "../components/Header.svelte";
    import { onMount } from "svelte";
    import { getDatabase, ref, onValue } from "firebase/database";

    // const givenWords = ['CHIHUAHUA', 'BULLDOG', 'TERRIER',
    // 'COLLIE', 'SHEPHERD', 'BOXER', 'HOUND',  'BEAGLE', 
    // 'CORGI', 'ROTTWEILER', 'PINSCHER', 'DALMATIAN', 
    // 'SETTER', 'MASTIFF', 'DACHSHUND']

    import { originGameSet } from "../assets/gameset";
    const db = getDatabase();
    const lineRef = ref(db, "line/");

    // 반응형 변수 선언. 자동으로 업데이트 됨
    $: allCreatedGame = originGameSet;
    console.log('game', allCreatedGame)

    // 화면이 렌더링될 때마다 onValue 호출될 수 있도록 하는 게 onMount
    onMount(() => {
        onValue(lineRef, (snapshot) => {
        const data = snapshot.val();
        allCreatedGame = allCreatedGame.concat(Object.values(data)); // 최신 업로드글이 위로
        console.log('all', allCreatedGame)
        });
    });

    export let params;
    const givenWords = allCreatedGame[Number(params.id)]['wordList'];

    // 현재 시간 간단히 표시할 수 있음
    let hour = new Date().getHours();
    let min = new Date().getMinutes();

</script>

<Header />
<div class='game'>
    <h2>Subject</h2>
    <div class="info-bar__time">{hour}:{min}</div>

    <div class="playbox">
      <GridBox givenWords={givenWords}></GridBox>
      <div class="word-list">
        {#each givenWords as val}
            <div>{val}</div>
        {/each}
      </div>
    </div>

</div>
