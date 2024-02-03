<script>
    export let givenWords;

    // min <= x <= max 범위에서 난수 추출
    function extractRandom(min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    // 단어들 중 가장 긴 길이
    const maxLen = Math.max.apply(null, givenWords.map(val => val.length));
    // 가로와 세로 길이
    const minWidth = 10;
    const rowLen = Math.max(minWidth, maxLen + 5);
    const colLen = Math.max(minWidth, maxLen + 5);

    function getRandomColor() {
	    // return "#" + Math.floor(Math.random() * 16777215).toString(16);
        let sampleColor = ['#FFE7E7', '#FFB0B0', '#C7C8CC', '#B7E5B4', '#FFEAA7', '#DCFFB7', '#E6A4B4',
         '#D5F0C1', '#AAD7D9', '#F6B17A', '#F3CCF3'];
        return sampleColor[extractRandom(0, sampleColor.length - 1)]
    }

    function positionOfSequence (seq, shape, index, wordLength, startRowIndex, startColIndex) {
        // 순방향이면
        let pos;
        if (seq) {
            if (shape === 'horizon') { pos = `${startRowIndex},${startColIndex + index}`; }
            else if (shape === 'vertical') { pos = `${startRowIndex + index},${startColIndex}`; }
            else { pos = `${startRowIndex + index},${startColIndex + index}`; }
        }
        // 역방향이면
        else {
            if (shape === 'horizon') {
                pos = `${startRowIndex},${startColIndex + wordLength - index - 1}`;
            }
            else if (shape === 'vertical') {
                pos = `${startRowIndex + wordLength - index - 1},${startColIndex}`;
            }
            else {
                pos = `${startRowIndex + wordLength - index - 1},${startColIndex + wordLength - index - 1}`;
            }
        }
        return pos;
    }

    /* 단어 배치 */
    function AssignWordPlace (wordList) {

        let positionArray = {};
        let correctPosition = {};
        let colorList = {};
        let wordInfo = {}; // 행렬 (인덱스는 0부터 시작)

        // 길이가 긴 단어부터 배열
        wordList.sort((x, y)=>y.length-x.length);

        wordList.map((word, wordIndex) => {
            
            /* 단어 하나 */
            let pointArray = []; // 위치 좌표들 배열
            let wordPlaceArray = []; // 단어들 배열
            let errorExist = true;
            let startRowIndex;
            let startColIndex;
            let sequence; // 순방향, 역방향
            let position;
            let shape = ['diagonal', 'vertical', 'horizon'][extractRandom(0, 2)]; // 모양
            let selectColor = getRandomColor();
            
            
            // 잘못 겹치는 것이 없을 때까지 반복
            let cnt = 0;
            while (errorExist) {
                console.log(`==== ${cnt} ${word} 진행중 ${shape}====`);

                errorExist = false;
                sequence = extractRandom(0, 2) === 1 ? true : false; // 순방향, 역방향
                
                startRowIndex = extractRandom(0, rowLen - word.length);
                startColIndex = extractRandom(0, colLen - word.length);
                if (shape === 'horizon') {
                    startRowIndex = extractRandom(0, rowLen - 1);
                } else if (shape === 'vertical') {
                    startColIndex = extractRandom(0, colLen - 1);
                }

                for (let letterIndex=0; letterIndex<word.length; letterIndex++) {
                    position = positionOfSequence(sequence, shape, letterIndex, word.length, startRowIndex, startColIndex);
                    if (positionArray[position]!== undefined && positionArray[position] !== word[letterIndex]) {
                        console.log('position', position, positionArray[position], word[letterIndex])
                        errorExist = true;
                        break;
                    }
                }
                cnt++;
            }

            // 잘못 겹치는 것이 없으면 while문 빠져나와서 필요한 곳에 정보 추가
            [...word].map((letter, letterIndex) => {
                position = positionOfSequence(sequence, shape, letterIndex, word.length, startRowIndex, startColIndex);
                positionArray[position] = letter;
                correctPosition[position] = letter;
                colorList[position] = selectColor;

                pointArray.push(position) // { word : ['0,1', '0,2', ...]}
                wordPlaceArray.push()
            })

            wordInfo[word] = {
                shape: shape,
                points: pointArray,
                clear: false,
                direction: sequence,
            }
        })


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

        return [positionArray, correctPosition, colorList, wordInfo];
    }

    let styleOn = false;
    let [wordPositionList, correctPosition, colorList, wordInfo] = AssignWordPlace(givenWords);

    /* 드래그 이벤트 */
    let draggElement = [];
    let correctWords = [];

    const dragHandler = (event) => {
        let dataIndex = event.target.getAttribute('data-index');
        let itemBox = document.querySelector(`.item-box[data-index="${dataIndex}"]`);
        try {
            itemBox.classList.add('drag-change');
        } catch {}
        draggElement.push(dataIndex);
    }

    const dragOverHandler = (event) => {
        let checkCorrect = false;
        let dragSet = new Set(draggElement);
        let dragCorrectArray = [...dragSet].filter((element) => {
            return element !== undefined && element !== null;
        });

        for (let wd of Object.keys(wordInfo)) {
            if (JSON.stringify(wordInfo[wd].points.sort()) == JSON.stringify(dragCorrectArray.sort())) {
                checkCorrect = true;
                correctWords.push(wd);
                break
            };
        }

        if (!checkCorrect) {
            dragCorrectArray.map((dataIndex) => {
                let itemBox = document.querySelector(`.item-box[data-index="${dataIndex}"]`);
                try {
                    itemBox.classList.remove('drag-change');
                } catch {}
            })
            dragCorrectArray = [];
        }
        draggElement = [];

        if (JSON.stringify(correctWords.sort()) === JSON.stringify(givenWords)) {
            alert("모든 정답을 맞췄습니다.")
        }
    }


    /* 정답 확인 찬스 */
    function answerCheck () {
        styleOn = !styleOn;
    }

    function answerOnCheck(rowIdx, colIdx, styleOn) {
        return Object.keys(correctPosition).includes(`${rowIdx},${colIdx}`) && styleOn;
    }

    // console.log('최종', positionArray);
    console.log('correct', correctPosition);
    console.log('wordinfo', wordInfo);
</script>




<div class='grid-box' style:grid-template-columns={`repeat(${rowLen}, 1fr)`}>
    {#each Array(rowLen) as _, rowIdx}
        {#each Array(colLen) as _, colIdx}
            <button 
            class='item-box'
            data-index={`${rowIdx},${colIdx}`} 
            on:dragenter={dragHandler} 
            on:dragend={dragOverHandler} 
            draggable="true"
            style={`background-color: ${answerOnCheck(rowIdx, colIdx, styleOn) ? colorList[`${rowIdx},${colIdx}`]: ''}`}
            >
                <div class='item'>{wordPositionList[`${rowIdx},${colIdx}`]}</div>
            </button>
        {/each}
    {/each}
</div>
<button on:click={answerCheck}>정답 확인</button>