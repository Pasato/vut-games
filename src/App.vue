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

const toDrawRepresentation = (board) =>   
  board.flat().map(x => x.pos);

export default {
  name: 'app',
  created() {
    this.board = Array.from(Array(boardSize), 
      (_row, k) => Array.from(Array(boardSize),
      (_col, j) => ({pos: [k, j], val: undefined})
    ));
    const [[firstRow, firstCol], [sndRow, sndCol]] = 
      draw2(toDrawRepresentation(this.board));

    this.board[firstRow][firstCol].val = 2;
    this.board[sndRow][sndCol].val = 2;
  },
  mounted() {
    document.body.addEventListener('keypress', (e) => console.log(e))
  },
  render() {
    return (
      <div style={boardStyle}>
        {this.board.map((rows) => 
          rows.map(({val}) => 
          <div style={cellStyle}>
            {val}
          </div>
          )
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
