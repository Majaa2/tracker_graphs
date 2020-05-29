<template>
  <v-app style="background-color:lightblue">
    <v-content >
      <v-row >
        <v-col cols="12" class="d-flex align-center justify-center">
          <v-btn-toggle
            v-model="selected"
            rounded
          >
            <v-btn>
              <span >Ukupni rezultati</span>
            </v-btn>
            <v-btn>
              <span >Podjela</span>
            </v-btn>
            <v-btn>
              <span >Vrste varanja</span>
            </v-btn>
          </v-btn-toggle>
        </v-col>
      </v-row>
      <v-row>
      <v-col cols="12" v-if="selected===0" class="d-flex align-center justify-center">
        <span>Ukupni rezultati testa</span>
      </v-col>
      <v-col cols="6" v-if="selected===1" class="d-flex align-center justify-center">S prepisivanjem</v-col>
      <v-col cols="6" v-if="selected===1" class="d-flex align-center justify-center">Bez prepisivanja</v-col>
       <v-col cols="3" v-if="selected===2" class="d-flex align-center justify-center">Chrome</v-col>
        <v-col cols="3" v-if="selected===2" class="d-flex align-center justify-center">Firefox/Opera</v-col>
         <v-col cols="3" v-if="selected===2" class="d-flex align-center justify-center">Mobitel</v-col>
    </v-row>
        <v-row style="height: 80%" v-show="selected===0" class="justify-center">
      <v-col cols="10">
        <apexchart width="100%" height="80%" type="bar" :options="options" :series="series"></apexchart>
      </v-col>
    </v-row>
    </v-content>
      <v-row v-show="selected===1">
      <v-col cols="6">
        <apexchart width="100%" height="80%" type="bar" :options="optionsCheaters" :series="seriesCheaters"></apexchart>
      </v-col>
      <v-col cols="6">
        <apexchart width="100%" height="80%" type="bar" :options="optionsNon" :series="seriesNon"></apexchart>
      </v-col>
    </v-row>
    <v-row style="height: 80%" v-show="selected===2" class="justify-start">
      <v-col cols="3">
        <apexchart width="100%" height="40%" type="bar" :options="optionsChromeCheaters" :series="seriesChrome"></apexchart>
      </v-col>
       <v-col cols="3">
        <apexchart width="100%" height="40%" type="bar" :options="optionsOtherCheaters" :series="seriesother"></apexchart>
      </v-col>
       <v-col cols="3">
        <apexchart width="100%" height="40%" type="bar" :options="optionsMobileCheaters" :series="seriesMobile"></apexchart>
      </v-col>
       <v-col cols="3">
        <apexchart width="100%" height="40%" type="donut" :options="optionsDonut" :series="seriesDonut"></apexchart>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import podatci from './data/podatci.json'
export default {
  name: 'App',

  computed:{
    cheatersChromeInitial(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Chrome'){
          testRes.push(std.inicijalni)
        }
      })
      return testRes
    },
    cheatersChromeFinal(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Chrome'){
         testRes.push(std.zavrsni)
        }
      })
      return testRes
    },
    cheatersOtherInitial(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Firefox/Opera'){
          testRes.push(std.inicijalni)
        }
      })
      return testRes
    },
    cheatersOtherFinal(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Firefox/Opera'){
         testRes.push(std.zavrsni)
        }
      })
      return testRes
    },
    cheatersMobileInitial(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Mobitel'){
          testRes.push(std.inicijalni)
        }
      }) 
      return testRes
    },
    cheatersMobileFinal(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Mobitel'){
         testRes.push(std.zavrsni)
        }
      })
      return testRes
    },
    studentData(){
      return podatci.studenti
    },
    cheatersInitial(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated){
          testRes.push(std.inicijalni)
        }
      })
      return testRes
    },
    cheatersFinal(){
      let testRes = []
       this.studentData.filter(std =>{
        if(std.cheated){
         testRes.push(std.zavrsni)
        }
      })
      return testRes
    },
    nonCheatersInitial(){
      let testRes = []
       this.studentData.filter(std =>{
        if(!std.cheated){
          testRes.push(std.inicijalni)
        }
      })
      return testRes
    },
    nonCheatersFinal(){
      let testRes = []
       this.studentData.filter(std =>{
        if(!std.cheated){
         testRes.push(std.zavrsni)
        }
      })
      return testRes
    },
    initialTest(){
      let testRes = []
      this.studentData.filter(std =>{
        testRes.push(std.inicijalni)
      })
      return testRes
    },
    finalTest(){
      let testRes = []
      this.studentData.filter(std =>{
        testRes.push(std.zavrsni)
      })
      return testRes
    },
        seriesChrome(){
      return [{
        name: 'Inicijalni',
        data: this.cheatersChromeInitial
      },{
        name: 'Završni',
        data: this.cheatersChromeFinal
      }]
    },
        seriesother(){
      return [{
        name: 'Inicijalni',
        data: this.cheatersOtherInitial
      },{
        name: 'Završni',
        data: this.cheatersOtherFinal
      }]
    },
        seriesMobile(){
      return [{
        name: 'Inicijalni',
        data: this.cheatersMobileInitial
      },{
        name: 'Završni',
        data: this.cheatersMobileFinal
      }]
    },
    series(){
      return [{
        name: 'Inicijalni',
        data: this.initialTest
      },{
        name: 'Završni',
        data: this.finalTest
      }]
    },
    seriesCheaters(){
      return [{
        name: 'Inicijalni',
        data: this.cheatersInitial
      },{
        name: 'Završni',
        data: this.cheatersFinal
      }]
    },
    seriesNon(){
      return [{
        name: 'Inicijalni',
        data: this.nonCheatersInitial
      },{
        name: 'Završni',
        data: this.nonCheatersFinal
      }]
    },
    seriesDonut(){
      let series = []

      series.push(this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Chrome'){
         return std
        }
      }).length)
      series.push(this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Firefox/Opera'){
         return std
        }
      }).length)
      series.push(this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Mobitel'){
         return std
        }
      }).length)

      return series
    },
    options() {
      let studnets = []
      this.studentData.filter(std =>{
        studnets.push(std.id)
      })
      return {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: studnets
        }
      }
      },
            optionsChromeCheaters() {
      let studnets = []
      this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Chrome'){
          studnets.push(std.id)
        }
      })
      return {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: studnets
        }
      }
      },
    optionsOtherCheaters() {
      let studnets = []
      this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Firefox/Opera'){
          studnets.push(std.id)
        }
      })
      return {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: studnets
        }
      }
      },
    optionsMobileCheaters() {
      let studnets = []
      this.studentData.filter(std =>{
        if(std.cheated && std.type === 'Mobitel'){
          studnets.push(std.id)
        }
      })
      return {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: studnets
        }
      }
      },
      optionsDonut() {
      return{
        labels: ['Chrome', 'Firefox/Opera', 'Mobitel']
        
        } 
      },
      optionsCheaters() {
      let studnets = []
      this.studentData.filter(std =>{
        if(std.cheated){
          studnets.push(std.id)
        }
      })
      return {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: studnets
        }
      }
      },
      optionsNon() {
      let studnets = []
      this.studentData.filter(std =>{
        if(!std.cheated){
          studnets.push(std.id)
        }
      })
      return {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: studnets
        }
      }
      }
  },
  data: () => ({
    selected: 0
  })
};
</script>
