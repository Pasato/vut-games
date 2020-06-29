<script>

const boardSize = 4;

const boardStyle = {
  display: 'grid',
  margin: '0 auto',
  position: 'relative',
  top: '200px',
  width: `${boardSize * 100 + (boardSize-1) * 10}px`,
  gridTemplateColumns: `repeat(${boardSize}, 100px)`,
  'column-gap': '10px',
  'row-gap': '10px'
};

const cellStyle = {
  display: 'flex',
  'font-size': '3em',
  'font-weight': 'bolder',
  background: '#355C7D',
  width: '100px',
  height: '100px',
  'justify-content': 'center',
  'align-items': 'center',
}

const matched = x => ({
  on: () => matched(x),
  otherwise: () => x,
})

const match = x => ({  
  on: (pred, fn) => (pred(x) ? matched(fn(x)) : match(x)),
  otherwise: fn => fn(x),
})

const drawRandom = (noDrawn) => (available) => {
  const indices = [];
  while (indices.length < noDrawn) {
    const randomInd = Math.floor(Math.random() * available.length);
    if (!indices.includes(randomInd)) {
      indices.push(randomInd);
    }
  }
  return available.filter((_, i) => indices.includes(i));
}

//const draw1 = drawRandom(1);
const draw2 = drawRandom(2);

const getEmptyIndices = (board) => 
  board.reduce((acc, row, rowInd) => 
    [
      ...acc, 
      ...row.filter(col => !col).map((_col, colInd) => [rowInd, colInd])
    ]
  ,[]);


export default {
  name: 'app',
  data() {
    return {
      board: Array.from(Array(boardSize), 
        () => Array.from(Array(boardSize),
        () => undefined))
    }
  },
  created() {
    const [[firstRow, firstCol], [sndRow, sndCol]] = 
      draw2(getEmptyIndices(this.board));
    
    this.setVal(firstRow, firstCol, 2);
    this.setVal(sndRow, sndCol, 2);
  },
  methods: {
    setVal: function(row, col, val) {
      const newRow = this.board[row].slice(0);
      newRow[col] = val;
      this.$set(this.board, row, newRow);
    }

  },
  mounted() {
    document.body.addEventListener('keyup', (e) => 
      match(e)
      .on(({key}) => key === 'ArrowUp', () => {
        for (let k=0;k<boardSize;k++) {
          for (let j=0;j<boardSize;j++) {
            if (this.board[k][j]) {
              for (let l=k-1;l>=0;l--) {
                const [swapTo, swapFrom] = [l, l+1];
                const [swapToVal, swapFromVal] = 
                  [this.board[swapTo][j], this.board[swapFrom][j]];

                if (!swapToVal && swapFromVal) {
                  this.setVal(swapTo, j, swapFromVal);
                  this.setVal(swapFrom, j, undefined)
                } else if (swapToVal === swapFromVal) {
                  this.setVal(swapTo, j, swapFromVal ** 2);
                  this.setVal(swapFrom, j, undefined)
                }
              }

            }
          }
        }
        console.log(this.board);
      })
      .otherwise(() => {})
    )
  },
  render() {
    return (
      <div style={boardStyle}>
        {this.board.map((row) => 
          row.map(val => (
          <div style={cellStyle}>
            {val}
          </div>
          ))
        )}
      </div>
    )
  }
};
</script>

<style>
body {
  margin: 0;
  width: 100%;
  height: 100vh;
  background: #F8B195;
}
#board {
  display: 'grid';
  gridTemplateColumns: 
  
}
</style>
