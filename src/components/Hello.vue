<template>

    <div class="chess">
    <!--<p>  {{a}}</p>-->
      <template v-for="(i,x) in pointObj">
          <template v-for="(v,y) in i">
            <i @click="setPoint(x,y)" :class="v==1?'b':v==2?'w':''"> {{x}},{{y}},{{v}}</i>
            <br v-if="y == 15">
        </template>
      </template>

    </div>

</template>

<script>

  import '../css/base.css'

export default {
  name: 'hello',
  data () {
    return {
      my:[],
      turn:'my',
      len:15,
      classObj:{},
      pointObj:[],
      isOver:false,
      machine:[],
      myS:0,
      userScore:0,
      aiScore:0,
      a:[[1,2],[3,4]]
    }
  },

  computed:{

  },
  created(){
    let pointObj  = [];
    let len =15;
    for(let x=1; x<=len; x++){
      pointObj[x] = []
      for(let y=1; y<=len; y++){
        pointObj[x][y]=0
      }
    }

    this.pointObj = pointObj;
  },
  methods:{
    /**
     * 下棋  x,y坐标
     * @param x
     * @param y
     * @returns {boolean}
     */
    setPoint(x,y){

        if(this.isOver){
           return false
        }

        if(this.pointObj[x][y]!=''){
            return false
        }

       if(this.turn=='my'){

         this.pointObj[x].splice(y,1,1);

         this.score(x,y);
         this.isGameOver(x,y);
         this.turn = 'machine';
         this.ai();

       }


    },

    score(x,y){
      let max = 0;  //是否赢了
      let flag;
      let tempXIndex = x;
      let tempYIndex = y;

      let score = this.turn==='my'?this.userScore:this.aiScore;

      // 三维数组记录横向，纵向，左斜，右斜的移动
      let dir = [
        // 横向
        [ [ -1, 0 ], [ 1, 0] ],
        // 竖着
        [ [  0, -1 ], [ 0, 1 ] ],
        // 左斜
        [ [  -1, -1 ], [ 1, 1 ] ],
        // 右斜
        [ [  1, -1 ], [ -1, 1 ] ]

      ];
      let [alive2,sleep2,alive3,sleep3,alive4,sleep4,alive5] = [0,0,0,0,0,0,0];  //多个

      for (let i = 0; i < 4; i++) {
        let count = 1;
        let isAlive = 0;  //边界是否有对手的棋子

        //j为0,1分别为棋子的两边方向，比如对于横向的时候，j=0,表示下棋位子的左边，j=1的时候表示右边
        for (let j = 0; j < 2; j++) {
          flag = true;
          /**
           while语句中为一直向某一个方向遍历
           有相同颜色的棋子的时候，Count++
           否则置flag为false，结束该该方向的遍历
           **/
          while (flag) {
            tempXIndex = tempXIndex + dir[i][j][0];
            tempYIndex = tempYIndex + dir[i][j][1];
            if(tempXIndex===0||tempYIndex===0) break;
            //当前点的5个点范围内去计算
            if (Math.abs(tempXIndex-x)>4||Math.abs(tempYIndex-y)>4){
              flag = false;
              break;
            }
            if ((this.pointObj[tempXIndex][tempYIndex] === this.pointObj[x][y])) {
              count++;

            } else if(this.pointObj[tempXIndex][tempYIndex]===0){

            }else{
              isAlive++
              flag = false;
            }

          }
          tempXIndex = x;
          tempYIndex = y;
        }

       if(count===2){
         if(isAlive===0){
            alive2++
            console.log('------活2-------')
         }else if(isAlive===1){
            sleep2++
           console.log('------眠2-------')
         }else if(isAlive===2){

           console.log('------死2-------')
         }

     }
       if(count===3){
        if(isAlive===0){
            alive3++
            console.log('------活3-------')
        }else if(isAlive===1){
            sleep3++
            console.log('------眠3-------')
        }else if(isAlive===2){
//            score+=1
            console.log('------死3-------')
        }

      }
       if(count===4){
        if(isAlive===0){
          alive4++
          console.log('------活4-------')
        }else if(isAlive===1){
          sleep4++
          console.log('------眠4-------')
        }else if(isAlive===2){
//          score+=1
          console.log('------死4-------')
        }

      }

        if(count>=5){
          alive5++;
          break;
        }

      }
      if(alive5===1){
        score+=1000000
      }
      //活4双眠4 眠4活3 双活4
      if(alive4===2||alive4===1||sleep4===2||(sleep4===1&&alive3===1)||(sleep4===1&&alive3===2)){
          score+=10000
      }
      // 双活3
      if(alive3===2){
        score+=5000
      }
      //眠4
      if(sleep4===1){
        score+=500
        if(sleep3===1){
          score+=500
        }
        if(sleep3===2){
          score+=4000
        }
        if(alive2===1){
          score+=100
        }
        if(alive2===2){
          score+=500
        }
        if(sleep2===1){
          score+=50
        }
        if(sleep2===2){
          score+=100
        }
      }
      //活3
      if(alive3===1){
        score+=100
        if(sleep3===1){
          score+=100
        }
        if(sleep3===2){
          score+=500
        }
        if(alive2===1){
          score+=50
        }
        if(alive2===2){
          score+=100
        }
        if(sleep2===1){
          score+=10
        }
        if(sleep2===2){
          score+=50
        }
      }
      //双眠3
      if(sleep3===2){
        score+=100
        if(alive2===1){
          score+=100
        }
        if(alive2===2){
          score+=200
        }
        if(sleep2===1){
          score+=20
        }
        if(sleep2===2){
          score+=100
        }
      }
      //眠3
      if(sleep3===1){
        score+=50
        if(alive2===1){
          score+=50
        }
        if(alive2===2){
          score+=100
        }
        if(sleep2===1){
          score+=10
        }
        if(sleep2===2){
          score+=50
        }
      }
      //活2
      if(alive2===3){
        score+=100
      }
      if(alive2===2){
        score+=30
        if(sleep2===1){
          score+=10
        }

      }
      if(alive2===1){
        score+=10
        if(sleep2===1){
          score+=10
        }
        if(sleep2===2){
          score+=20
        }
      }
      if(sleep2===3&&sleep2===2){
        score+=10
      }
      if(sleep2===1){
        score+=5
      }

      if(this.turn==='my'){
        this.userScore = score
      }else{
        this.aiScore = score
      }
      console.log('user得分---'+this.userScore)
      console.log('ai得分---'+this.aiScore)

    },

    ai(){

      for(let x=1; x<=len; x++){

        for(let y=1; y<=len; y++){
//          this.pointObj[x][y]
        }
      }



    },
    /**
     * 每一步判断游戏结果
     * @param x
     * @param y
     */
    isGameOver(x,y){
      let max = 0;  //是否赢了
      let flag;
      let tempXIndex = x;
      let tempYIndex = y;
      // 三维数组记录横向，纵向，左斜，右斜的移动
      let dir = [
        // 横向
        [ [ -1, 0 ], [ 1, 0] ],
       // 竖着
        [ [  0, -1 ], [ 0, 1 ] ],
        // 左斜
        [ [  -1, -1 ], [ 1, 1 ] ],
         // 右斜
        [ [  1, -1 ], [ -1, 1 ] ]

       ];

      for (let i = 0; i < 4; i++) {
       let count = 1;
        //j为0,1分别为棋子的两边方向，比如对于横向的时候，j=0,表示下棋位子的左边，j=1的时候表示右边
        for (let j = 0; j < 2; j++) {
          flag = true;
          /**
           while语句中为一直向某一个方向遍历
           有相同颜色的棋子的时候，Count++
           否则置flag为false，结束该该方向的遍历
           **/
          while (flag) {
            tempXIndex = tempXIndex + dir[i][j][0];
            tempYIndex = tempYIndex + dir[i][j][1];
            if(tempXIndex===0||tempYIndex===0) break;
            if ((this.pointObj[tempXIndex][tempYIndex] === this.pointObj[x][y])) {
              count++;

            } else
              flag = false;
            }
            tempXIndex = x;
            tempYIndex = y;
        }

        if (count >= 5) {
          max = 1;
          break;
        } else
          max = 0;
      }
      if (max === 1){
        if(this.turn=='my'){
          alert('你赢了')
        }else{
          alert('ai赢了')
        }

        return true;
      }


    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  [v-cloak] {
    display: none;
  }
  p{
    color: #000;
    font-size: 20px;
  }
  #app {

    margin: 0 auto;
    text-align: center;
  }
  h1 {
    font-size: 20px;
    text-align: center;
  }
  .chess {
    position: relative;
    left: 50%;
    width: 1000px;
    margin: 0 0 30px -500px;
    font-size: 0;
  }

  /* 坐标 */
  i {
    position: relative;
    display: inline-block;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    cursor: pointer;
    text-align: left;
  }
  /* 白棋 */
  i.w,
  i.w::before,
  i.w::after {
    background-color: #fff;
    border-color: #fff;
  }

  /* 黑棋 */
  i.b,
  i.b::before,
  i.b::after {
    background-color: #333;
  }
  /* - 横线 */
  i::before,
  i::after {
    content: ' ';
    position: absolute;
    top: 20px;
    width: 40px;
    height: 0;
    border-top: 1px solid #333;
  }
  /* | 竖线 */
  i::after {
    top: 0;
    right: 20px;
    width: 0;
    height: 40px;
    border-top: 0 none;
    border-right: 1px solid #333;
  }
  button {
    margin-right: 5px
  }
</style>
