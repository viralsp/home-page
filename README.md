# home-page

#donot go through this

const Canvatable = () => {
    const canvaref = useRef()
    var ctx;
    let columnWidth =100 ; 
    let rowHeight =30 ;
    async function table(selectedCell=null){
        ctx.restore();
        ctx.clearRect(0,0,canvaref.current.width, canvaref.current.height);
        console.log("table");
        // let columnWidth = Math.floor(canvaref.current.width/dataColumns.length);
        console.log(columnWidth);
        // let rowHeight = Math.floor(canvaref.current.height/rows.length);


        //header data and table
        for(let i=0;i<columnWidth;i++){
          ctx.save();
          ctx.rect(i*columnWidth,0,columnWidth,rowHeight); //x position y position width height
          ctx.clip()
          ctx.fillStyle="black";    
          ctx.font=`bold ${15}px Arial`
          ctx.fillText(dataColumns[i] , i*columnWidth+10,rowHeight-10);
          ctx.restore();
          ctx.moveTo(i*columnWidth,0);
          ctx.lineTo(i*columnWidth,canvaref.current.height);
          ctx.stroke();
          
        }

        //cell data
        for (let i = 0; i < dataColumns.length; i++) {
          for (let j = 0; j < rows.length; j++) {
            // console.log(i,j,rows[j][dataColumns[i]]);
            // ctx.beginPath()
            ctx.save();
            ctx.rect(i * columnWidth,(j + 1) * rowHeight,columnWidth,rowHeight);
            ctx.clip();
            ctx.font = `${15}px Arial`;
            ctx.fillText(rows[j][dataColumns[i]],i * columnWidth + 10,(j + 2) * rowHeight - 10);
            ctx.moveTo(i*columnWidth,0);
            ctx.lineTo(i*columnWidth,canvaref.current.height);
            ctx.stroke();
            ctx.restore();
          }
        }
        ctx.save();
        console.log(selectedCell)
    }
    function handleClick(e){
      let xcord = Math.floor(e.clientX/columnWidth) //horizontal mouse click control
      let ycord = Math.floor(e.clientY/rowHeight) //vertocal mouse roll
      if(ycord>0){
          ycord--;
          console.log("cell position : " + ycord + " " + xcord)
          console.log("Cell data : ", rows[ycord][dataColumns[xcord]])
          table({selectedCell:{row:ycord,col:xcord}});
      }
    }
    useEffect(() => {
        canvaref.current.width = dataColumns.length * columnWidth;
        canvaref.current.height = rows.length * rowHeight;
        ctx=canvaref.current.getContext("2d");
        table();
    }, []);
    return (
        <div className='table'>
            <canvas id="myCanvas" ref={canvaref} onClick={handleClick}></canvas>
        </div>
    );
}

export default Canvatable;
