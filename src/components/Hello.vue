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

         this.turn = 'machine'

       }else if(this.turn=='machine'){

         this.pointObj[x].splice(y,1,2);

         this.turn = 'my'
       }
       this.isGameOver(x,y)
    },
    ai(){

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
          alert('赢了')
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
