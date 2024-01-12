<script>
    import Header from "../components/Header.svelte";
    import { getDatabase, ref, push } from "firebase/database";

    let title;
    let description;
    let wordList = [];
    let subject;
    let inputWord = '';

    const db = getDatabase();

    async function submitGameSetting() {
        console.log(wordList);
        // 5글자 이상인지 확인
        const wordCheck = wordList.map((word) => word.length >= 5);

        if (wordCheck.includes(false)) {
            alert("단어는 5글자 이상이어야 합니다.")
            wordList = [];
        } else if (wordList.length < 10) {
            alert("단어는 10개 이상이어야 합니다.")
            wordList = [];
        } else {
            console.log('ok')
            push(ref(db, "line/"), {
                title, // title:title을 생략
                description,
                wordList,
                subject,
            });
            window.location.hash = "/";
        }

    }

    const handleWordList = (e) => {
        wordList.push(e.target.value);
    }

</script>
<Header />
<div class='admin'>
    <form id="admin-form" on:submit|preventDefault={submitGameSetting}>

        <div class="formbox">
            <label for="subject">Subject</label>
            <input type="text" id="subject" required bind:value={subject}/>
        </div>

        <div class="formbox">
            <label for="title">Title</label>
            <input type="text" id="title" required bind:value={title}/>
        </div>

        <div class="formbox">
            <label for="des">Description</label>
            <textarea id="des" rows="2" cols="33" bind:value={description}></textarea>
        </div>

        <ul class="word-list-form">
        <label for="word1">Word List</label>
        <p>Between 10 and 30 words. Puzzles are randomly generated using a selection of your words at play time.</p>
        <div class="word-list-grid">
            <!-- admin -->
            {#each [...Array(parseInt('30'))] as item, index}
                <li id={`word${index}`}>
                    <input type="text" value={inputWord} on:change={handleWordList} />
                </li>
            {/each}
        </div>
        </ul>


        <button type="submit" class="submit-btn" >Submit</button>

    </form>
</div>