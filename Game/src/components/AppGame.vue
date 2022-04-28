<template>
    <div>
        <div class="mss">
            <ion-label color="success" class="win" v-if="aWinner">
                {{ winMss }}
                <ion-icon :src="getIcon('gift')"/> 
            </ion-label>
            <ion-label class="status" v-else>{{getStatus()}}</ion-label>
           
        </div>
        <div v-for="(row, x) in matrix" :key="row" > 
            <div class="row" >
                <ion-button v-for="(button, y) in row" :key="button"
                @click="play(x, y)"
                class="square" 
                :disabled="matrix[x][y] || aWinner"
                style="width:px ">
                <ion-label>{{matrix[x][y]}}</ion-label>
                </ion-button>
            </div>
        </div>
        <div class="mss" >
             <ion-button 
             :disabled="!showReset" 
             @click="reset()"><ion-icon 
             :src="getIcon('reload')"/> </ion-button>
        </div>
    </div>
</template>

<script>
import { 
  IonButton, IonLabel, IonIcon 
 } from '@ionic/vue';
export default {
    name: 'TicTac',
    components:{IonButton, IonLabel, IonIcon },
    data() {
        return {
            matrix: [ [null, null, null], [null, null, null], [null, null, null] ],
            value: 'X',
            aWinner: false,
            winMss: '',
            showReset: false,
        }
    },
   methods: {
        play(x, y){
        this.matrix[x][y] = this.value
        this.value==='X' ? this.value='O' : this.value='X'
        this.showReset = true
        this.verifyingWinner()
    },
    reset(){
        this.matrix = [ [null, null, null], [null, null, null], [null, null, null] ]
        this.value = 'X'
        this.aWinner = false;
        this.showReset = false;       
    },
    
    getStatus(){       
      return this.matrix.filter(ele => ele.includes(null)).length ? `Next Player: ${this.value}` : 'Tied Game'
    },
    oneWins(winner){
        this.aWinner = true
        this.winMss = `${winner} WINS`
    },
    elementToVerify(element){
        const haveX = (currentValue) => currentValue === 'X';
        const haveO = (currentValue) => currentValue === 'O'; 
        element.every(haveX) ? this.oneWins('X') : element.every(haveO) ? this.oneWins('O') : ''
    },
    verifyingWinner(){ 
        //horizontal            
        this.matrix.forEach( element => {this.elementToVerify(element) })
        //vertical
        for (let index = 0; index < 3; index++) {
           const col = this.matrix.map( ele => ele[index])
           this.elementToVerify(col)
        }
        //transversal
         const down = [this.matrix[0][0], this.matrix[1][1], this.matrix[2][2]]
         this.elementToVerify(down)
         const up = [this.matrix[2][0], this.matrix[1][1], this.matrix[0][2]]
         this.elementToVerify(up)
    }
   },
}
</script>
<style scoped>
    .square{
        width: 100px;
        height: 100px;
        background: lightgrey;
        margin: 10px;
    }
    ion-label{
        font-size: x-large;
    }
    .mss{
        display: flex;
        align-items: center;
        justify-content: space-around;
        margin: 0 auto;
    }
    .win{
        font-weight: bold;
        font-size: x-large;
    }
</style>