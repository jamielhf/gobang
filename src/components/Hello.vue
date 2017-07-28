<template>

    <div class="chess">
    <!--<p>  {{a}}</p>-->
      <template v-for="(i,x) in pointObj">
          <template v-for="(v,y) in i">
            <i @click="setPoint(x,y)" :class="v"> {{x}},{{y}},{{v}}</i>
            <br v-if="y == 14">
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
    for(let x=0; x<len; x++){
      pointObj[x] = []
      for(let y=0; y<len; y++){
        pointObj[x][y]=''
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

         this.pointObj[x].splice(y,1,'b');

         this.turn = 'machine'

       }else if(this.turn=='machine'){

         this.pointObj[x].splice(y,1,'w');

         this.turn = 'my'
       }
       this.isGameOver(x,y)
    },
    //每一步判断游戏结果
    isGameOver(x,y){
       let t = 'b' , step = 1;
       if(this.turn=='my'){
         t = 'w';
       }
       //纵向判断
       for(let i=1;x-i>=0;i++){
          if(this.pointObj[x-i][y] == t){
              step++
          }else{
              break
          }
       }
      for(let i=1;x+i<=14;i++){
        if(this.pointObj[x+i][y] == t){
          step++
        }else{
          break
        }
      }

      if(step>=5){
        this.isOver = true
        alert('win')
        return
      }else{
          step = 1;
      }

      //横向判断
      for(let i=1;y-i>=0;i++){
        if(this.pointObj[x][y-i] == t){
          step++
        }else{
          break
        }
      }
      for(let i=1;y+i<=14;i++){
        if(this.pointObj[x][y+i] == t){
          step++
        }else{
          break
        }
      }
      if(step>=5){
        this.isOver = true
        alert('win')
        return
      }else{
        step = 1;
      }

      for(let i=1;x-i>=0,y-i>=0;i++){
        if(this.pointObj[x-i][y-i] == t){
          step++
        }else{
          break
        }
      }
      for(let i=1;x+i<=14,y+i<=14;i++){
        if(this.pointObj[x+i][y+i] == t){
          step++
        }else{
          break
        }
      }
      if(step>=5){
        this.isOver = true
        alert('win');
        return
      }else{
        step = 1;
      }

      for(let i=1;x-i>=0,y+i<=14;i++){
        if(this.pointObj[x-i][y+i] == t){
          step++
        }else{
          break
        }
      }
      for(let i=1;x+i<=14,y-i>=0;i++){
        if(this.pointObj[x+i][y-i] == t){
          step++
        }else{
          break
        }
      }
      if(step>=5){
        this.isOver = true
        alert('win')
        return
      }else{
        step = 1;
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
