<script>
    import GridBox from "../components/GridBox.svelte"
    import Header from "../components/Header.svelte";
    import { onMount } from "svelte";
    import { getDatabase, ref, onValue } from "firebase/database";

    const givenWords = ['CHIHUAHUA', 'BULLDOG', 'TERRIER',
    'COLLIE', 'SHEPHERD', 'BOXER', 'HOUND',  'BEAGLE', 
    'CORGI', 'ROTTWEILER', 'PINSCHER', 'DALMATIAN', 
    'SETTER', 'MASTIFF', 'DACHSHUND']


    // 현재 시간 간단히 표시할 수 있음
    let hour = new Date().getHours();
    let min = new Date().getMinutes();

    // 반응형 변수 선언. 자동으로 업데이트 됨
    $: items = [];

    const db = getDatabase();
    const lineRef = ref(db, "line/");

    // 화면이 렌더링될 때마다 onValue 호출될 수 있도록 하는 게 onMount
    onMount(() => {
        onValue(lineRef, (snapshot) => {
        const data = snapshot.val();
        items = Object.values(data); // items 변수 업데이트
        });
    });
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
