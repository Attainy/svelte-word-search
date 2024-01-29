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

    let positionArray = [];
    let wordInfo = {};

    function AssignWordPlace (wordList) {
        wordList.map((word, index) => {
            let pointArray = []; // 위치 좌표들 배열
            let direction = extractRandom(0, 2) === 1 ? true : false; // 순방향, 역방향
            let startRowIndex;
            let startColIndex;

            // 수평 배열 => 행은 무작위 추출, 열은 글자 길이 고려
            if (directionsArray[index] === 'horizon') {
                startRowIndex = extractRandom(0, rowLen - 1);
                startColIndex = extractRandom(0, colLen - word.length + 1);

                word.map((letter, index) => {
                    pointArray.append(`${}`)
                })
            }


            wordInfo[word] = {
                shape: directionsArray[index],
                points: pointArray,
                clear: false,
                direction: 
            }
        })
    }







    // // 단어 랜덤으로 추출해서 시작 위치 지정하기
    // function randomExtractWord (shape) {
    //     // 단어 랜덤 추출
    //     word = givenWords[extractRandom(0, givenWords.length - 1)];

    //     // 시작 위치
    //     startRowIndex = extractRandom(0, rowLen - word.length + 1);
    //     startColIndex = extractRandom(0, colLen - word.length + 1);

    //     if (shape === "horizon") {
    //         startRowIndex = extractRandom(0, rowLen - 1);
    //         while (rowIndexArray.includes(startRowIndex)) {
    //         startRowIndex = extractRandom(0, rowLen - 1)
    //         }
    //     } else if (shape === "vertical") {
    //         startColIndex = extractRandom(0, colLen - 1);
    //         while (colIndexArray.includes(startColIndex)) {
    //         startColIndex = extractRandom(0, colLen - 1)
    //         }
        
    //       // 대각선
    //     } else {
    //         startRowIndex = extractRandom(0, rowLen - 1);
    //         while (rowIndexArray.includes(startRowIndex)) {
    //         startRowIndex = extractRandom(0, rowLen - 1)
    //         }
    //         startColIndex = extractRandom(0, colLen - 1);
    //         while (colIndexArray.includes(startColIndex)) {
    //         startColIndex = extractRandom(0, colLen - 1)
    //         }
    //     } 
    //     return [word, startRowIndex, startColIndex];
    // };


    // function checkArray (shape) {
    //     [word, startRowIndex, startColIndex] = randomExtractWord(shape);
    //     // 정방향
    //     [...word].map((letter, index) => {
    //         rowIndex = shape === "horizon" ? startRowIndex : startRowIndex + index;
    //         colIndex = shape === "vertical" ? startColIndex : startColIndex + index;

    //         // console.log('중간', box[rowIndex][colIndex]);
    //         let boxLetter = box?.[rowIndex]?.[colIndex];
    //         if (boxLetter === 0 || boxLetter === letter) check = true;
    //         else check = false;
    //     })
    //     return [check, word, startRowIndex, startColIndex];
    // };

    // // 격자에 단어 배열하기
    // function placeWord (shape) {
    //     [check, word, startRowIndex, startColIndex] = checkArray(shape);

    //     while (!check) {
    //         checkArray(shape);
    //     };

    //     wordInfo[word] = {
    //         'clear': false,
    //         'shape': shape, 
    //         'points': [],
    //         // 'points': [[startRowIndex, startColIndex]],
    //     };

    //     [...word].map((letter, index) => {
    //         rowIndex = shape==="horizon" ? startRowIndex : startRowIndex + index;
    //         colIndex = shape==="vertical" ? startColIndex : startColIndex + index;

    //         box[rowIndex][colIndex] = letter;
    //         wordInfo[word]['points'].push([rowIndex, colIndex]);
    //     })
    // };

    // // console.log(wordInfo);
    // console.log(box);
    // console.log(wordInfo['ICE']);

    // 실행
    // [...Array(horizontalCount)].map((v, i) => placeWord('horizon'));
    // [...Array(verticalCount)].map((v, i) => placeWord('vertical'));
    // [...Array(diagonalCount)].map((v, i) => placeWord('diagonal'));

    // console.log('box', box)
    // const items = box.reduce(function (acc, cur) {
    //     return acc.concat(cur);
    // });
    // console.log('items', items)
    // console.log(wordInfo)

    // 나머지 칸 랜덤 글자로 채우기
    const alphabet ='ABCDEFGHIJKLMNOPQRSTUVWXYZ';
    // for (let i in items) {
    //     if (items[i] === 0) {
    //         items[i] = alphabet[extractRandom(0, alphabet.length -1)];
    //     };
    // };


    // 정답 글자
    // correctWords = ['BOXER']
    // const applyStyleCheck = (rowIdx, colIdx) => {
    // let styleCheck = false;
    // Object.keys(wordInfo).map((word) => {
    //     correctWords.map((word) => {
    //     if (wordInfo[word]['clear'] == true) {
    //     wordInfo[word].points.map((el) => {
    //         if (el == [rowIdx, colIdx]) styleCheck = true;
    //     });
    //     };
    // })
    // return styleCheck;
    // }

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
</script>



<div class='grid-box' style:grid-template-columns={`repeat(${rowLen}, 1fr)`}>
    {#each items as val, index}
        <button class="item-box" data-index={`${index}`} on:dragenter={dragHandler} on:dragend={dragOverHandler} draggable="true">
            <!-- <div class="item">{val}</div> -->
        </button>
    {/each}
</div>

<style>



</style>