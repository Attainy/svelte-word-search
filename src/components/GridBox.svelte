<script>
    export let givenWords;

    // min <= x <= max 범위에서 난수 추출
    function extractRandom(min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    const rowIndexArray = [];
    const colIndexArray = [];

    let word;
    let startRowIndex;
    let startColIndex;
    let rowIndex;
    let colIndex;
    let check;

    // 단어들 중 가장 긴 길이
    const maxLen = Math.max.apply(null, givenWords.map(val => val.length));

    // 가로와 세로 길이
    const minWidth = 10;
    const rowLen = extractRandom(Math.min(minWidth, maxLen + 2), Math.max(minWidth, maxLen + 2))
    const colLen = extractRandom(Math.min(minWidth, maxLen + 2), Math.max(minWidth, maxLen + 2))

    // 가로, 세로, 대각선 각 모양별 개수 (최소 2개씩 설정)
    let [horizontalCount, verticalCount, diagonalCount] = [2, 2, 2];
    horizontalCount = Math.floor(givenWords.length/2);
    verticalCount = givenWords.length - horizontalCount;
    let directionsArray = [];
    for (let i=0; i<horizontalCount; i++) {
        directionsArray.push('horizon');
    }
    for (let i=0; i<verticalCount; i++) {
        directionsArray.push('vertical');
    }
    for (let i=0; i<diagonalCount; i++) {
        directionsArray.push('diagonal');
    }
    directionsArray.sort(() => Math.random() - 0.5); // 무작위 배열



    // 행렬 (인덱스는 0부터 시작)
    let positionArray = {}; // ex. {'1,3': 'W'}
    let correctPosition = {}; // ex. {'1,3': 'W'}
    let wordInfo = {};

    function positionOfSequence (seq, index, wordLength, startRowIndex, startColIndex) {
        // 순방향이면
        let pos;
        if (seq) {
            if (directionsArray[index] === 'horizon') pos = `${startRowIndex},${startColIndex + index}`;
            else if (directionsArray[index] === 'vertical') pos = `${startRowIndex + index},${startColIndex}`;
            else pos = `${startRowIndex + index},${startColIndex + index}`;
        }
        // 역방향이면
        else {
            if (directionsArray[index] === 'horizon') pos = 
            `${startRowIndex},${startColIndex + wordLength - index - 1}`;
            else if (directionsArray[index] === 'vertical') pos = 
            `${startRowIndex + wordLength - index - 1},${startColIndex}`;
            else pos = 
            `${startRowIndex + wordLength - index - 1},${startColIndex + wordLength - index - 1}`;
        }
        return pos;
    }


    function AssignWordPlace (wordList) {
        wordList.map((word, index) => {
            
            /* 단어 하나 */
            let pointArray = []; // 위치 좌표들 배열
            let wordPlaceArray = []; // 단어들 배열
            let errorExist = false;
            let startRowIndex;
            let startColIndex;
            let sequence; // 순방향, 역방향
            let position;
            let wordLength = word.length;
            

            // 잘못 겹치는 것이 없을 때까지 반복
            let cnt = 0;
            while (!errorExist && cnt < 10) {
            // for (let j=0; j<10; j++) {
                
                console.log('================= 진행중 =================');
                
                errorExist = false;
                sequence = extractRandom(0, 2) === 1 ? true : false; // 순방향, 역방향
                
                if (directionsArray[index] === 'horizon') {
                    startRowIndex = extractRandom(0, rowLen - 1);
                    startColIndex = extractRandom(0, colLen - word.length + 1);
                } else if (directionsArray[index] === 'vertical') {
                    startColIndex = extractRandom(0, colLen - 1);
                    startRowIndex = extractRandom(0, rowLen - word.length + 1);
                } else {
                    startRowIndex = extractRandom(0, rowLen - word.length + 1);
                    startColIndex = extractRandom(0, colLen - word.length + 1);
                }

                [...word].map((letter, index) => {
                    position = positionOfSequence(sequence, index, wordLength, startRowIndex, startColIndex);

                    // 위치 같은데 글자 다르면
                    if (Object.keys(positionArray).includes(position) && (positionArray[position] !== letter)) {
                        errorExist = true;
                    }
                    console.log(errorExist);
                })

                cnt++;
            }

            // 잘못 겹치는 것이 없으면 while문 빠져나와서 필요한 곳에 정보 추가
            [...word].map((letter, index) => {
                position = positionOfSequence(sequence, index, wordLength, startRowIndex, startColIndex);
                positionArray[position] = letter;
                correctPosition[position] = letter;

                // pointArray.push(position)
                // wordPlaceArray.push()
            })

            // wordInfo[word] = {
            //     shape: directionsArray[index],
            //     points: pointArray,
            //     clear: false,
            //     direction: sequence,
            // }

        })
    }

    console.log('중간')
    console.log(positionArray);

    // 나머지 칸 랜덤 글자로 채우기
    const alphabet ='ABCDEFGHIJKLMNOPQRSTUVWXYZ';
    
    for (let row=0; row<rowLen; row++) {
        for (let col=0; col<colLen; col++) {
            let tempPos = `${row},${col}`;
            if (!Object.keys(positionArray).includes(tempPos)) {
                positionArray[tempPos] = alphabet[extractRandom(0, alphabet.length -1)];
            }
        }
    }

    

    /* 드래그 이벤트 */
    let draggElement = [];

    const dragHandler = (event) => {
        let dataIndex = event.target.getAttribute('data-index');
        let itemBox = document.querySelector(`.item-box[data-index="${dataIndex}"]`);
        try {
            itemBox.classList.add('drag-change');
            draggElement.push(dataIndex);
        } catch {

        }
    }

    const dragOverHandler = (event) => {
        draggElement.map((dataIndex) => {
            let itemBox = document.querySelector(`.item-box[data-index="${dataIndex}"]`);
            try {
                itemBox.classList.remove('drag-change');
            } catch {

            }
        })
    }


    let styleOn = false;
    

    /* 정답 확인 */
    function answerCheck () {
        for (let row=0; row<rowLen; row++) {
            for (let col=0; col<colLen; col++) {
                let tempPos = `${row},${col}`;
                
            }
        }
    }


    AssignWordPlace(givenWords);

    console.log('최종')
    console.log(positionArray);
    console.log('correct', correctPosition);

</script>




<div class='grid-box' style:grid-template-columns={`repeat(${rowLen}, 1fr)`}>
    <!-- {#each [...Array((rowLen-1)*(colLen-1))] as val, index} -->
    {#each Array(rowLen) as _, rowIdx}
        {#each Array(colLen) as _, colIdx}
            <button class={`item-box ${Object.keys(correctPosition).includes(`${rowIdx},${colIdx}`) ? 'answer' : ''}`} data-index={`${rowIdx},${colIdx}`} on:dragenter={dragHandler} on:dragend={dragOverHandler} draggable="true">
                <div class='item'>{positionArray[`${rowIdx},${colIdx}`]}</div>
            </button>
        {/each}
    {/each}
</div>
<button on:click={answerCheck}>정답 확인</button>

<style>

.answer {
    background-color: yellow;
}

</style>