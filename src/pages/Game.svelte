<script>
    import GridBox from "../components/GridBox.svelte"
    import Header from "../components/Header.svelte";
    // const givenWords = ['CHIHUAHUA', 'BULLDOG', 'TERRIER',
    // 'COLLIE', 'SHEPHERD', 'BOXER', 'HOUND',  'BEAGLE', 
    // 'CORGI', 'ROTTWEILER', 'PINSCHER', 'DALMATIAN', 
    // 'SETTER', 'MASTIFF', 'DACHSHUND']
  
    const allCreatedGame = JSON.parse(localStorage.getItem("allCreatedGame"));

    export let params;
    console.log(typeof(Number(params.id)))
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
      <GridBox givenWords={allCreatedGame[Number(params.id)]['wordList']}></GridBox>
      <div class="word-list">
        {#each givenWords as val}
            <div>{val}</div>
        {/each}
      </div>
    </div>

</div>
