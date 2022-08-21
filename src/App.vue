<template>
  <div class="wrapper">
    
    <!-- <button  @click="test()" >test</button>&nbsp; -->
    <button style="margin-left:250px" @click="generate()" >generate</button>

    <div class="playingArea">
      <template v-for="(item,index) in dotsMap" :key="index">
        <template v-if="index<200">

          <div v-if="item" class="square" :style="getLocation(index)"></div>
          <div v-else class="empty" :style="getEmpty(index)"></div>
        </template>
      </template>
    </div>

    <div class="nextBlocks">
      <div class="squareForNext" :style="styleForNextBlocks(0)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(1)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(2)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(3)"></div>

      <div class="squareForNext" :style="styleForNextBlocks(4)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(5)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(6)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(7)"></div>

      <div class="squareForNext" :style="styleForNextBlocks(8)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(9)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(10)"></div>
      <div class="squareForNext" :style="styleForNextBlocks(11)"></div>
      
    </div>

    <div class="controller">
      <button @click="moveTo('left')" v-on:keyup.enter="moveTo('left')">←</button>
      <button @click="moveTo('down')">↓</button>
      <button @click="moveTo('right')">→</button>
      &nbsp;&nbsp;

      <img @click="rotate()" style=" background-color:white; " src="../public/iconmonstr-refresh-1.svg" >
      <button  @click="downHard()">down</button>
      
      <!-- <button @click="moveTo('drop')">↓</button> -->
    </div>

    <div>
      <span  @keyup.enter="test()"></span>
    </div>

    <!-- <div>
      <input v-on:keyup.space="moveTo('left')"> <br>
    </div> -->

  </div>
</template>

<script>

export default {
  name: 'App',

  data(){
    return{
      hasGameStarted: false,
      fallingSpeed: 1500,
      dotsMap: [],
      nextBlocks: [],
      blockIndex: 0,

      current1: undefined,
      current2: undefined,
      current3: undefined,
      current4: undefined,
      currentColor: undefined,
      currentShape: undefined,
    }
  },

  methods:{
    sleep(ms) {
      return new Promise((resolve) => {
        setTimeout(resolve, ms);
      });
    },

    getLocation(index){
      let style = undefined
      
      let height  = index - (index % 10); // This will subtract off the last digit.
      height = height / 10;
      height = height * 20

      style = `bottom: ${height}px;`

      let left = +index.toString().split('').pop();
      left = left * 20

      style = style + ` left: ${left}px;`

      style = style + ` background: ${this.dotsMap[index]};`



      return style

      
    },
    getEmpty(index){
      let style = undefined
      
      let height  = index - (index % 10); // This will subtract off the last digit.
      height = height / 10;
      height = height * 20

      style = `bottom: ${height}px;`

      let left = +index.toString().split('').pop();
      left = left * 20

      style = style + ` left: ${left}px;`



      return style

      
    },
    styleForNextBlocks(num){
      let height 
      let left 
      let color
      if(num < 4){
        switch(this.nextBlocks[this.blockIndex]){

          case 6:

            color = '#3fff00'
            height = 2
            if(num == 0 || num == 1){
              height = 1
            }
            switch(num){
              case 0:
                left = 1.5
                break;

              case 1:
                left = 2.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 5:

            color = 'red'
            height = 2
            if(num == 0 || num == 1){
              height = 1
            }
            switch(num){
              case 0:
                left = 2.5
                break;

              case 1:
                left = 3.5
                break;

              
              case 2:
                left = 1.5
                break;

              case 3:
                left = 2.5
                break;
            }
            break;
          
          case 4:

            color = 'orange'
            height = 2
            if(num == 0){
              height = 1
            }
            switch(num){
              case 0:
                left = 3.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 3: 
            color = '#ff0090'
            height = 2
            if(num == 0){
              height = 1
            }

            switch(num){
              case 0:
                left = 1.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 2: 
            color = '#dda0dd'

            height = 2
            if(num == 0){
              height = 1
            }

            switch(num){
              case 0:
                left = 2.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 1:
            color = 'skyblue'

            height = 2

            switch(num){
              case 0:
                left = 1
                break;

              case 1:
                left = 2
                break;

              
              case 2:
                left = 3
                break;

              case 3:
                left = 4
                break;
            }

            break;
          
          case 0:
            color = 'yellow'

            if(num == 0 || num == 1){
              height = 2
            }else{
              height = 3
            }

            if(num == 0 || num == 2){
              left = 2
            }else{
              left = 3
            }

            break;





          

        }
        height = height * 10
        left = left * 10
        return  `top: ${height}px; left: ${left}px; background: ${color};` 
        
      }

      

      if(num < 8){
        num = num -4
        switch(this.nextBlocks[this.blockIndex+1]){

          case 6:

            color = '#3fff00'
            height = 2
            if(num == 0 || num == 1){
              height = 1
            }
            switch(num){
              case 0:
                left = 1.5
                break;

              case 1:
                left = 2.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 5:

            color = 'red'
            height = 2
            if(num == 0 || num == 1){
              height = 1
            }
            switch(num){
              case 0:
                left = 2.5
                break;

              case 1:
                left = 3.5
                break;

              
              case 2:
                left = 1.5
                break;

              case 3:
                left = 2.5
                break;
            }
            break;
          
          case 4:

            color = 'orange'
            height = 2
            if(num == 0){
              height = 1
            }
            switch(num){
              case 0:
                left = 3.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 3: 
            color = '#ff0090'
            height = 2
            if(num == 0){
              height = 1
            }

            switch(num){
              case 0:
                left = 1.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 2: 
            color = '#dda0dd'

            height = 2
            if(num == 0){
              height = 1
            }

            switch(num){
              case 0:
                left = 2.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 1:
            color = 'skyblue'

            height = 2

            switch(num){
              case 0:
                left = 1
                break;

              case 1:
                left = 2
                break;

              
              case 2:
                left = 3
                break;

              case 3:
                left = 4
                break;
            }

            break;
          
          case 0:
            color = 'yellow'

            if(num == 0 || num == 1){
              height = 2
            }else{
              height = 3
            }

            if(num == 0 || num == 2){
              left = 2
            }else{
              left = 3
            }

            break;





          

        }
        height = (height * 10) + 50
        left = left * 10
        return  `top: ${height}px; left: ${left}px; background: ${color};` 

      }

      if(num < 12){
        num = num -8
        switch(this.nextBlocks[this.blockIndex+2]){

          case 6:

            color = '#3fff00'
            height = 2
            if(num == 0 || num == 1){
              height = 1
            }
            switch(num){
              case 0:
                left = 1.5
                break;

              case 1:
                left = 2.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 5:

            color = 'red'
            height = 2
            if(num == 0 || num == 1){
              height = 1
            }
            switch(num){
              case 0:
                left = 2.5
                break;

              case 1:
                left = 3.5
                break;

              
              case 2:
                left = 1.5
                break;

              case 3:
                left = 2.5
                break;
            }
            break;
          
          case 4:

            color = 'orange'
            height = 2
            if(num == 0){
              height = 1
            }
            switch(num){
              case 0:
                left = 3.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 3: 
            color = '#ff0090'
            height = 2
            if(num == 0){
              height = 1
            }

            switch(num){
              case 0:
                left = 1.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 2: 
            color = '#dda0dd'

            height = 2
            if(num == 0){
              height = 1
            }

            switch(num){
              case 0:
                left = 2.5
                break;

              case 1:
                left = 1.5
                break;

              
              case 2:
                left = 2.5
                break;

              case 3:
                left = 3.5
                break;
            }
            break;

          case 1:
            color = 'skyblue'

            height = 2

            switch(num){
              case 0:
                left = 1
                break;

              case 1:
                left = 2
                break;

              
              case 2:
                left = 3
                break;

              case 3:
                left = 4
                break;
            }

            break;
          
          case 0:
            color = 'yellow'

            if(num == 0 || num == 1){
              height = 2
            }else{
              height = 3
            }

            if(num == 0 || num == 2){
              left = 2
            }else{
              left = 3
            }

            break;





          

        }
        height = (height * 10) + 100
        left = left * 10
        return  `top: ${height}px; left: ${left}px; background: ${color};` 

      }

      // if(num < 12){

      // }
      
    },

    generate(){

      let randomNum = this.nextBlocks[this.blockIndex]

      switch(randomNum){
        case 0: 
          this.current1 = 195
          this.current2 = 196
          this.current3 = 185
          this.current4 = 186
          this.currentColor = 'yellow'
          this.currentShape = 'square'
          break;

        case 1: 
          this.current1 = 193
          this.current2 = 194
          this.current3 = 195
          this.current4 = 196
          this.currentColor = 'skyblue'
          this.currentShape = 'line1'
          break;
        
        case 2: 
          this.current1 = 195
          this.current2 = 184
          this.current3 = 185
          this.current4 = 186
          this.currentColor = '#dda0dd'
          this.currentShape = 't-shape1'
          break;

        case 3: 
          this.current1 = 193
          this.current2 = 183
          this.current3 = 184
          this.current4 = 185
          this.currentColor = '#ff0090'
          this.currentShape = 'j-shape1'
          break;

        case 4: 
          this.current1 = 195
          this.current2 = 183
          this.current3 = 184
          this.current4 = 185
          this.currentColor = 'orange'
          this.currentShape = 'l-shape1'
          break;

        case 5: 
          this.current1 = 195
          this.current2 = 196
          this.current3 = 184
          this.current4 = 185
          this.currentColor = 'red'
          this.currentShape = 's-shape1'
          break;

        case 6: 
          this.current1 = 194
          this.current2 = 195
          this.current3 = 185
          this.current4 = 186
          this.currentColor = '#3fff00'
          this.currentShape = 'z-shape1'
          break;
        
      }

      this.blockIndex++ 
      this.moveTheBlock()

    },

    async moveTheBlock(){
      this.dotsMap[this.current1] = this.currentColor
      this.dotsMap[this.current2] = this.currentColor
      this.dotsMap[this.current3] = this.currentColor
      this.dotsMap[this.current4] = this.currentColor


      let count = 0
      while(count < 20){

        await this.sleep(500)
        if(this.shouldIStop()){
          
          this.current1 = undefined
          this.current2 = undefined
          this.current3 = undefined
          this.current4 = undefined
          this.currentColor = undefined
          this.currentShape = undefined

          // judge to remove the line or not await for some time
          let flag = false
          while(!flag){
            if(!this.judge()){
              flag = true
            }
          }
          

          this.generate()

          return
        }

        

        this.dotsMap[this.current1] = undefined
        this.dotsMap[this.current2] = undefined
        this.dotsMap[this.current3] = undefined
        this.dotsMap[this.current4] = undefined

        this.current1 = this.current1 -10  
        this.current2 = this.current2 -10
        this.current3 = this.current3 -10
        this.current4 = this.current4 -10


        this.dotsMap[this.current1] = this.currentColor
        this.dotsMap[this.current2] = this.currentColor
        this.dotsMap[this.current3] = this.currentColor
        this.dotsMap[this.current4] = this.currentColor
        
        count++

      }
    },
    shouldIStop(){
      if(this.current1 < 10 || this.current2< 10 || this.current3 < 10 || this.current4 < 10){
        return true
      }

      if(this.currentShape == 'square'){
        if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }

      if(this.currentShape == 'line1'){
        if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'line2'){
        if(this.dotsMap[this.current4 - 10] !== undefined ){
          return true
        }
      }

      if(this.currentShape == 't-shape1'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 't-shape2'){
        if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 't-shape3'){
        if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 't-shape4'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }


      if(this.currentShape == 'j-shape1'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'j-shape2'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'j-shape3'){
        if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'j-shape4'){
        if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }

      if(this.currentShape == 'l-shape1'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'l-shape2'){
        if(this.dotsMap[this.current3 - 10] !== undefined ||  this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'l-shape3'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'l-shape4'){
        if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }

      if(this.currentShape == 's-shape1'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 's-shape2'){
        if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }

      if(this.currentShape == 'z-shape1'){
        if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined){
          return true
        }
      }
      if(this.currentShape == 'z-shape2'){
        if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4 - 10] !== undefined ){
          return true
        }
      }

      return false
    },

    moveTo(direction){
      if(!this.current1) return
      this.dotsMap[this.current1] = undefined
      this.dotsMap[this.current2] = undefined
      this.dotsMap[this.current3] = undefined
      this.dotsMap[this.current4] = undefined

      switch(direction){
        case 'left':
          if(!this.canYouDoTheMove('left')) break;
          
          this.current1 = this.current1 -1 
          this.current2 = this.current2 -1
          this.current3 = this.current3 -1
          this.current4 = this.current4 -1
          break;

        case 'down':
          if(!this.canYouDoTheMove('down')) break;
          
          this.current1 = this.current1 -10
          this.current2 = this.current2 -10
          this.current3 = this.current3 -10
          this.current4 = this.current4 -10
          break;

        case 'right':
          if(!this.canYouDoTheMove('right')) break;

          this.current1 = this.current1 +1
          this.current2 = this.current2 +1
          this.current3 = this.current3 +1
          this.current4 = this.current4 +1
          break;
        
        

        
      }   

      this.dotsMap[this.current1] = this.currentColor
      this.dotsMap[this.current2] = this.currentColor
      this.dotsMap[this.current3] = this.currentColor
      this.dotsMap[this.current4] = this.currentColor
    },  
    canYouDoTheMove(direction){
      let num1 = +this.current1.toString().split('').pop();
      let num2 = +this.current2.toString().split('').pop();
      let num3 = +this.current3.toString().split('').pop();
      let num4 = +this.current4.toString().split('').pop();

      switch(direction){
        case 'left':
          if(num1 == 0 || num2 == 0 || num3 == 0  || num4 == 0 ){
            return false 
          }

          if(this.currentShape == 'square'){
            if(this.dotsMap[this.current1- 1] !== undefined  ||this.dotsMap[this.current3- 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'line1'){
            if(this.dotsMap[this.current1- 1] !== undefined ){
              return false 
            }
          }
          if(this.currentShape == 'line2'){
            if(this.dotsMap[this.current1- 1] !== undefined ){
              return false 
            }
          }

          if(this.currentShape == 't-shape1'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape2'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape3'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape4'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'j-shape1'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape2'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current3- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape3'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape4'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined || this.dotsMap[this.current3- 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'l-shape1'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'l-shape2'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined || this.dotsMap[this.current3- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'l-shape3'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'l-shape4'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current3- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 's-shape1'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current3- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 's-shape2'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'z-shape1'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current3- 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'z-shape2'){
            if(this.dotsMap[this.current1- 1] !== undefined || this.dotsMap[this.current2- 1] !== undefined || this.dotsMap[this.current4- 1] !== undefined){
              return false 
            }
          }

          break;

        case 'right':
          if(num1 == 9 || num2 == 9|| num3 == 9  || num4 == 9 ){
            return false 
          }

          if(this.currentShape == 'square'){
            if(this.dotsMap[this.current2+ 1] !== undefined  ||this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'line1'){
            if(this.dotsMap[this.current4 + 1] !== undefined ){
              return false 
            }
          }

          if(this.currentShape == 'line2'){
            if(this.dotsMap[this.current4 + 1] !== undefined ){
              return false 
            }
          }

          if(this.currentShape == 't-shape1'){
            if(this.dotsMap[this.current1 + 1] !== undefined || this.dotsMap[this.current4 +1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape2'){
            if(this.dotsMap[this.current1 + 1] !== undefined || this.dotsMap[this.current3 +1] !== undefined || this.dotsMap[this.current4 +1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape3'){
            if(this.dotsMap[this.current3 + 1] !== undefined || this.dotsMap[this.current4 +1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape4'){
            if(this.dotsMap[this.current1 + 1] !== undefined || this.dotsMap[this.current3 +1] !== undefined || this.dotsMap[this.current4 +1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'j-shape1'){
            if(this.dotsMap[this.current1+ 1] !== undefined || this.dotsMap[this.current4 + 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape2'){
            if(this.dotsMap[this.current2 + 1] !== undefined || this.dotsMap[this.current3+ 1] !== undefined || this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape3'){
            if(this.dotsMap[this.current3 + 1] !== undefined || this.dotsMap[this.current4 + 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape4'){
            if(this.dotsMap[this.current1+ 1] !== undefined || this.dotsMap[this.current2+ 1] !== undefined || this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'l-shape1'){
            if(this.dotsMap[this.current1+ 1] !== undefined || this.dotsMap[this.current4 + 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'l-shape2'){
            if(this.dotsMap[this.current1 + 1] !== undefined || this.dotsMap[this.current2+ 1] !== undefined || this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'l-shape3'){
            if(this.dotsMap[this.current3 + 1] !== undefined || this.dotsMap[this.current4 + 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'l-shape4'){
            if(this.dotsMap[this.current2+ 1] !== undefined || this.dotsMap[this.current3+ 1] !== undefined || this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 's-shape1'){
            if(this.dotsMap[this.current2+ 1] !== undefined || this.dotsMap[this.current4 + 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 's-shape2'){
            if(this.dotsMap[this.current1 + 1] !== undefined || this.dotsMap[this.current3+ 1] !== undefined || this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'z-shape1'){
            if(this.dotsMap[this.current2+ 1] !== undefined || this.dotsMap[this.current4 + 1] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'z-shape2'){
            if(this.dotsMap[this.current1 + 1] !== undefined || this.dotsMap[this.current3+ 1] !== undefined || this.dotsMap[this.current4+ 1] !== undefined){
              return false 
            }
          }

          break;

        case 'down':
          if(this.current1 < 10 || this.current2< 10 || this.current3 < 10 || this.current4 < 10){
            return false 
          }

          if(this.currentShape == 'square'){
            if(this.dotsMap[this.current3- 10] !== undefined  ||this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'line1'){
            if(this.dotsMap[this.current1- 10] !== undefined  ||this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined  ||this.dotsMap[this.current4 -10] !== undefined ){
              return false 
            }
          }
          if(this.currentShape == 'line2'){
            if(this.dotsMap[this.current4- 10] !== undefined  ){
              return false 
            }
          }

          if(this.currentShape == 't-shape1'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape2'){
            if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape3'){
            if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 't-shape4'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'j-shape1'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape2'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape3'){
            if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current2- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape4'){
            if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'j-shape1'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape2'){
            if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape3'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'j-shape4'){
            if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 's-shape1'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 's-shape2'){
            if(this.dotsMap[this.current2 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }

          if(this.currentShape == 'z-shape1'){
            if(this.dotsMap[this.current1 - 10] !== undefined || this.dotsMap[this.current3- 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }
          if(this.currentShape == 'z-shape2'){
            if(this.dotsMap[this.current3 - 10] !== undefined || this.dotsMap[this.current4- 10] !== undefined){
              return false 
            }
          }

          break;

        
      }   

      return true
    },

    judge(){
      let count = 0
      let flag = false
      while(count< 200){
        if(this.dotsMap[count] !== undefined && this.dotsMap[count+1] !== undefined && this.dotsMap[count+2] !== undefined && this.dotsMap[count+3] !== undefined && this.dotsMap[count+4] !== undefined && this.dotsMap[count+5] !== undefined && this.dotsMap[count+6] !== undefined && this.dotsMap[count+7] !== undefined && this.dotsMap[count+8] !== undefined && this.dotsMap[count+9] !== undefined){
          console.log('removing the line')
          this.dotsMap[count] = undefined
          this.dotsMap[count+1] = undefined
          this.dotsMap[count+2] = undefined
          this.dotsMap[count+3] = undefined
          this.dotsMap[count+4] = undefined
          this.dotsMap[count+5] = undefined
          this.dotsMap[count+6] = undefined
          this.dotsMap[count+7] = undefined
          this.dotsMap[count+8] = undefined
          this.dotsMap[count+9] = undefined
          
          flag= true
          this.dropEverything(count)
          return true
        }
        count = count+10
      }
      return flag
    },

    dropEverything(num){
      let count= num
      while(count < 200){
        this.dotsMap[count] = this.dotsMap[count+10]
        count++
      }
    },

    rotate(){
      let num
      // judge if you can rotate and then change the currentshape into new one
      switch(this.currentShape){
        case 'line1':
          if(this.current1 < 30) return
          if(this.dotsMap[this.current2-20] == undefined && this.dotsMap[this.current2-10] == undefined && this.dotsMap[this.current2+10] == undefined ){
            console.log('changing shape')
            this.currentShape = 'line2'
            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current2 +20
            this.current2 = this.current2 +10
            this.current3 = this.current2 -10
            this.current4 = this.current2 -20

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor
            // this.dotsMap[this.current4] = 'red'
          }else{
            console.log('cannot now')
          } break;

        case 'line2':

          num = +this.current1.toString().split('').pop();
          if(num > 7) return
          if(num < 1) return

          if(this.dotsMap[this.current3-1] == undefined && this.dotsMap[this.current3+1] == undefined && this.dotsMap[this.current3+2] == undefined ){
            console.log('changing shape')
            this.currentShape = 'line1'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current3 -1
            this.current2 = this.current3 
            this.current3 = this.current3 +1
            this.current4 = this.current3 +1

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor
          }else{
            console.log('cannot now')
          }
          break;


        case 't-shape1':

          if(this.current3 <10) return

          if(this.dotsMap[this.current3-10] == undefined ){
            console.log('changing shape')
            this.currentShape = 't-shape2'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current2 = this.current2 +1
            this.current3 = this.current3 + 1
            this.current4 = this.current2 - 10

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }else{
            console.log('cannot now')
          }

          break;

        case 't-shape2':
          num = +this.current1.toString().split('').pop();
          if(num < 1) return

          if(this.dotsMap[this.current3 +1] == undefined ){
            console.log('changing shape')
            this.currentShape = 't-shape3'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current2 -1
            // this.current2 = this.current1+1
            // this.current3 = this.current2+1
            // this.current4 = this.current2 - 10

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }else{
            console.log('cannot now')
          }

          break;

        case 't-shape3':

          if(this.dotsMap[this.current2 +10] == undefined ){
            console.log('changing shape')
            this.currentShape = 't-shape4'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current2 +10
            this.current2 = this.current2 -1
            this.current3 = this.current2+1
            // this.current4 = this.current3 +1

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }else{
            console.log('cannot now')
          }

          break;

        case 't-shape4':
          num = +this.current2.toString().split('').pop();
          if(num > 8 ) return

          if(this.dotsMap[this.current2 -1] == undefined ){
            console.log('changing shape')
            this.currentShape = 't-shape1'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            // this.current1 = this.current2 +10
            // this.current2 = this.current2 -1
            // this.current3 = this.current3 -1
            this.current4 = this.current3 +1

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }else{
            console.log('cannot now')
          }

          break;


        case 'j-shape1':

          if(this.dotsMap[this.current3 + 10] == undefined &&  this.dotsMap[this.current3 + 11] == undefined &&  this.dotsMap[this.current3 - 10] == undefined){
            console.log('changing shape')
            this.currentShape = 'j-shape2'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current3 +10
            this.current2 = this.current3 +11
            // this.current3 = this.current1 -10
            this.current4 = this.current3 -10

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;

        case 'j-shape2':

          if(this.dotsMap[this.current3 - 1] == undefined &&  this.dotsMap[this.current3 + 1] == undefined &&  this.dotsMap[this.current4 + 1] == undefined){
            console.log('changing shape')
            this.currentShape = 'j-shape3'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current3 -1
            this.current2 = this.current3
            this.current3 = this.current3 +1
            this.current4 = this.current3 - 10

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;
        
        case 'j-shape3':

          if(this.dotsMap[this.current2 + 10] == undefined &&  this.dotsMap[this.current2 -10] == undefined && this.dotsMap[this.current1 -10] == undefined){
            console.log('changing shape')
            this.currentShape = 'j-shape4'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current2 +10
            // this.current2 = this.current4 +10
            this.current3 = this.current2 -11
            this.current4 = this.current2 -10
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;
        
        case 'j-shape4':

          if(this.dotsMap[this.current3 + 10] == undefined &&  this.dotsMap[this.current4 +1] == undefined ){
            console.log('changing shape')
            this.currentShape = 'j-shape1'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current3 +10
            this.current2 = this.current3
            this.current3 = this.current3 +1
            this.current4 = this.current3 +1
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;


        case 'l-shape1':

          if(this.dotsMap[this.current3 + 10] == undefined &&  this.dotsMap[this.current3 - 10] == undefined &&  this.dotsMap[this.current4 - 10] == undefined){
            console.log('changing shape')
            this.currentShape = 'l-shape2'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current1 -1
            this.current2 = this.current2 +1
            this.current3 = this.current2 -10
            this.current4 = this.current3 +1

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;

        case 'l-shape2':

          if(this.dotsMap[this.current2 - 1] == undefined &&  this.dotsMap[this.current2 + 1] == undefined &&  this.dotsMap[this.current3 - 1] == undefined){
            console.log('changing shape')
            this.currentShape = 'l-shape3'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current2 -1
            // this.current2 = this.current3
            this.current3 = this.current2 +1
            this.current4 = this.current1 - 10

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;
        
        case 'l-shape3':

          if(this.dotsMap[this.current2 + 10] == undefined &&  this.dotsMap[this.current2 -10] == undefined && this.dotsMap[this.current2 -9] == undefined){
            console.log('changing shape')
            this.currentShape = 'l-shape4'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current1 +10
            this.current2 = this.current2 +10
            this.current3 = this.current2 -10
            this.current4 = this.current3 -10
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;
        
        case 'l-shape4':

          if(this.dotsMap[this.current3  -1] == undefined &&  this.dotsMap[this.current3 +1] == undefined &&  this.dotsMap[this.current3 +11] == undefined ){
            console.log('changing shape')
            this.currentShape = 'l-shape1'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current1 +2
            this.current2 = this.current3 -1
            // this.current3 = this.current3 +1
            this.current4 = this.current3 +1
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;


        case 's-shape1':

          if(this.dotsMap[this.current1 - 1] == undefined &&  this.dotsMap[this.current1 +9] == undefined){
            console.log('changing shape')
            this.currentShape = 's-shape2'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current1 +9
            this.current2 = this.current1 -10
            this.current3 = this.current2 +1
            this.current4 = this.current3 -10
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;
        
        case 's-shape2':
          num = +this.current3.toString().split('').pop();
          if(num > 8 ) return

          if(this.dotsMap[this.current3 + 1] == undefined &&  this.dotsMap[this.current4 - 1] == undefined ){
            console.log('changing shape')
            this.currentShape = 's-shape1'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current3
            this.current2 = this.current1+1
            this.current3 = this.current4 -1
            // this.current4 = this.current3 +1
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;


        case 'z-shape1':

          if(this.dotsMap[this.current2  +1] == undefined &&  this.dotsMap[this.current2 + 11] == undefined){
            console.log('changing shape')
            this.currentShape = 'z-shape2'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current1 +12 
            // this.current2 = this.current1 -10
            this.current3 = this.current2 +1
            this.current4 = this.current2 -10
            // this.current4 = this.current4 

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;
        
        case 'z-shape2':
          num = +this.current2.toString().split('').pop();
          if(num < 1 ) return

          if(this.dotsMap[this.current2 - 1] == undefined &&  this.dotsMap[this.current4 + 1] == undefined ){
            console.log('changing shape')
            this.currentShape = 'z-shape1'

            this.dotsMap[this.current1] = undefined
            this.dotsMap[this.current2] = undefined
            this.dotsMap[this.current3] = undefined
            this.dotsMap[this.current4] = undefined

            this.current1 = this.current2 -1
            // this.current2 = this.current1+1
            this.current3 = this.current2 -10
            // this.current4 = this.current3 +1
            this.current4 = this.current3 +1

            this.dotsMap[this.current1] = this.currentColor
            this.dotsMap[this.current2] = this.currentColor
            this.dotsMap[this.current3] = this.currentColor
            this.dotsMap[this.current4] = this.currentColor

            // this.dotsMap[this.current1] = this.currentColor
            // this.dotsMap[this.current2] = 'red'
            // this.dotsMap[this.current3] = 'green'
            // this.dotsMap[this.current4] = 'black'
          }
          break;


      }

      
    },

    test(){
      console.log('tuping')

    },

    downHard(){
      this.moveTo('down')
      this.moveTo('down')
      this.moveTo('down')
      this.moveTo('down')
    },


  },

  mounted(){
    console.clear()
    let count = 0
    while(count< 200){
      this.dotsMap.push(undefined)
      count++
    }

    count = 0
    // let max = 6
    let max = 6
    let min = 0
    // let min = 6
    min = Math.ceil(min);
    max = Math.floor(max);
    let randomIndex
    while(count< 20000){
      randomIndex = Math.floor(Math.random() * (max - min + 1)) + min;
      this.nextBlocks.push(randomIndex)
      count++
    }

    // console.log(this.nextBlocks)

    


  },
  created() {
    window.addEventListener('keydown', (e) => {
      if(e.key == 'a'){
        this.moveTo('left')
      }else if(e.key == 'd'){
        this.moveTo('right')
      }else if(e.key == 's'){
        this.moveTo('down')
      }else if(e.key == 'Enter'){
        if(this.hasGameStarted){
          this.rotate()
        }else{
          
          this.generate()
          this.hasGameStarted = true
        }
      }else if(e.key == ' '){
        this.moveTo('down')
        this.moveTo('down')
        this.moveTo('down')
        this.moveTo('down')
        return
      }
      
      return
      
      // if (e.key == 'Escape') {
      //   console.log('hey')
      // }
    });
  },
}

</script>

<style>
#app {
  touch-action: manipulation;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 20px; */
}

.wrapper{
  position: relative;
}

.playingArea{
  position: absolute;
  /* background: #00003B; */
  /* background: #4f86f7; */
  top:5px;
  left: 30px;

  height: 420px;
  width: 250px;
  /* border: 5px solid  MediumSlateBlue; */
}


.square{
  position:absolute;
  

  height: 16px;
  width: 16px;

  padding: 2px;

  /* background: red; */
  box-shadow:  inset 0 0 0 1px #00003B;
  /* background-color: #00003B; */

}

.empty{
  position:absolute;
  

  height: 16px;
  width: 16px;

  padding: 2px;

  /* background: red; */
  box-shadow:  inset 0 0 0 1px #00003B;
  background-color: #4166f5;
  /* background: white; */

}

.nextBlocks{
  position: absolute;
  background: #00003B;
  top:50px;
  right: 15px;

  height: 160px;
  width: 60px;
  /* border: 5px solid  MediumSlateBlue; */
}

.squareForNext{
  position:absolute;
  

  height: 10px;
  width: 10px;

  /* padding: 2px; */

  /* background: red; */
  /* box-shadow:  inset 0 0 0 px #00003B; */
  /* background-color: #00003B; */

}

.controller{
  position: absolute;
  background: #00003B;
  top :450px;
  left: 50%;
  transform: translateX(-50%);
}

.controller button{
  font-size: 50px;
}
</style>
