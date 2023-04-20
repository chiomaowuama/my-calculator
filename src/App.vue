<script setup>
import { computed } from '@vue/reactivity';
import { ref } from 'vue';

const currentTheme = ref(localStorage.getItem('theme-color'));
const output = ref('');
const previousValue = ref(null);
const operationFired = ref (false);


function switchTheme(theme){
  localStorage.setItem('theme-color', theme);
  currentTheme.value = theme;
}
function clearField(){
  output.value = '';
}
function getNumber(number){
  if(operationFired.value){
    output.value = ''
    operationFired.value = false;
  }
  output.value = `${output.value}${number}`;
  
}
function getDot(){
  if(output.value.indexOf('.') === -1){
    output.value = output.value+'.';
  }
}

function proccessOutput(string){
  if(string == 'add'){
    operation = (a,b) => {
      return parseFloat(a) + parseFloat(b);
    }
  }
  else if(string == 'subtract'){
    operation = (a,b) => {
    return parseFloat(a) - parseFloat(b);
  }
  }
  else if(string == 'divide'){
    operation = (a,b) => {
    return parseFloat(a) / parseFloat(b);
  }

  }
  else if(string == 'multiply'){
    operation = (a,b) => {
    return parseFloat(a) * parseFloat(b);
  }
  }
  previousValue.value = output.value;
  operationFired.value = true;
}
function updateOutput(){
  output.value = `${this.operation(previousValue.value, output.value)}`;
  previousValue.value = null;

}
function deleteNumber(){
output.value = output.value.slice(0,-1);
}
</script>

<template>
  <div class="body" :class="currentTheme" id="body">
    <!-- {{ currentTheme }} -->
    <div class="the-calculator">
      <div class="logo-and-theme">
        <div class="logo">
          <p>calc</p>
        </div>
        <div class="the-theme">
          <div class="numbers">
            <div class="numbers-small-container">
              <p class="each-numbers">1</p>
              <p class="each-numbers">2</p>
              <p class="each-numbers">3</p>
            </div>
          </div>
          <div class="below-numbers">
            <div class="the-text">
              <p>THEME</p>
            </div>
            
            <div class="theme-option">
              <div v-bind:class="[
                currentTheme === 'theme-white' ? 'active' : '',
                'theme-white',
              ]"
              class="radius-color three"              
              @click="switchTheme('theme-white')"
              id="theme-white"></div>
              <div v-bind:class="[
                currentTheme === 'theme-red' ? 'active' : '',
                'theme-red',
              ]"
              class="radius-color one"
              @click="switchTheme('theme-red')"
              id="theme-red">
              </div>
              <div v-bind:class="[currentTheme === 'theme-yellow' ? 'active' : '',
                'theme-yellow',
              ]"
              class="radius-color two" 
              @click="switchTheme('theme-yellow')" id="theme-yellow"></div>
            </div>
          </div>
        </div>
      </div>
      <div class="the-result-part" >
        <input type="text" class="result" :value="output||0"/> 
        <!-- :value="output||0" -->
      </div>
      <div class="buttons-and-all">
        <div class="buttons-part">
          <input type="button" class="btn-number" value="7" @click="getNumber('7')"/>
          <input type="button" class="btn-number"  value="8" @click="getNumber('8')"/>
          <input type="button" class="btn-number"  value="9" @click="getNumber('9')"/>
          <input type="button" class="btn-number reset unset"  value="DEL" @click="deleteNumber()">
          <input type="button" class="btn-number"  value="4" @click="getNumber('4')"/>
          <input type="button" class="btn-number"  value="5" @click="getNumber('5')"/>
          <input type="button" class="btn-number" value="6" @click="getNumber('6')"/>
          <input type="button" class="btn-number" value="+" @click="proccessOutput('add')"/>
          <input type="button" class="btn-number" value="1" @click="getNumber('1')"/>
          <input type="button" class="btn-number" value="2" @click="getNumber('2')"/>
          <input type="button" class="btn-number" value="3" @click="getNumber('3')"/>
          <input type="button" class="btn-number" value="-" @click="proccessOutput('subtract')"/>
          <input type="button" class="btn-number" value="." @click="getDot()"/>
          <input type="button" class="btn-number" value="0" @click="getNumber('0')"/>
          <input type="button" class="btn-number" value="/" @click="proccessOutput('divide')"/>
          <input type="button" class="btn-number" value="x" @click="proccessOutput('multiply')"/>
        </div>
        <div class="reset-equal">
          <input type="button" class="end-buttons reset" value="RESET" @click="clearField()">
          <input type="button" class="end-buttons equal-to" value="=" @click=" updateOutput()">
        </div>
    </div>
    </div>
  </div>
</template>

<style >
/* .App{
  width:100%;
} */
.body{
  /* border:2px solid red; */
  width:99.5%;
  background-color: hsl(222, 26%, 31%);
  min-height:99vh;
  color: hsl(0, 0%, 100%);
  display:flex;
  justify-content: center;
  align-items: center;
}
.the-calculator{
  width:28%;
  margin:auto;
  padding:20px 20px;
  box-shadow:  0 4px hsla(0, 1%, 63%, 0.062);
  transform: translateY(10px);
  /* border:2px solid green; */
}
/* buttons part */
.buttons-part{
  display:grid;
  grid-template-columns: repeat(4, 1fr);
  row-gap: 0.9rem;
  column-gap: 0.3rem;
  padding-left:2%;
  width:100%;
  /* border:2px solid blue;  */
}
.buttons-and-all{
  background-color: hsl(224, 36%, 15%);
  padding:20px 20px;
  border-radius: 10px;
  /* border:2px solid red; */
}
.btn-number{
  padding:10px;
  width:75%;
  border:2px;
  font-size: 15px;
  font-weight:700;
  font-family:Arial, Helvetica, sans-serif;
  border-radius:10px;
  /* border:2px solid green; */

}
.btn-number:hover{
  background-color:hsl(30, 25%, 89%);
  box-shadow:  0 2px hsl(28, 16%, 55%);
  transform: translateY(2px);
}
.reset-equal{
  /* border:2px solid red; */
  display:grid;
  grid-template-columns: 1fr 1fr;
  row-gap: 1rem;
  margin-top:4%;
  column-gap: 0.4rem;
  padding-left:2%;
  

}
.reset{
  background-color:  hsl(224, 28%, 35%);
  color:white;
}
.reset:hover{
  background-color:  hsl(225, 21%, 55%);
  box-shadow:  0 3px hsl(224, 28%, 35%);
  transform: translateY(2px);
  cursor:pointer;
  color:white;
}
.equal-to{
  background-color: hsl(6, 70%, 34%);
}
.equal-to:hover{
  background-color: hsl(6, 63%, 60%);
  box-shadow:  0 3px hsl(6, 70%, 34%);
  transform: translateY(2px);
  cursor:pointer;
  color:white;
}
.end-buttons{
  padding:12px;
  width:90%;
  border-radius:10px;
  border:none;
  font-size: 15px;
  font-weight:700;
  
}
/* the result part */
.the-result-part{
  /* border:2px solid green; */
  width:100%;
  border:none;
  margin:15px 0;
   
 
}
.result{
  /* border:2px solid green; */
  width:100%;
  background-color: hsl(224, 36%, 15%);
  font-size: 40px;
  font-weight: 600;
  padding:20px 0px;
  border-radius:7px;
  border:none;
  outline:none;
  color:hsl(0, 0%, 100%);
  text-align: right;
}
/* logo and themes */
.logo-and-theme{
  /* border:2px solid red; */
 display:grid;
 grid-template-columns: 8fr 4fr;
}
.numbers{
  display:flex;
  justify-content: right;
  align-content: center;
}
.numbers-small-container{

  width:50%;
  display:flex;
  justify-content:space-around;
}
.each-numbers{
  font-size: 8px;
  font-weight: 800;
  padding:0 8%;
  color:black;
  /* border:2px solid red; */
  
}
/* themes */
.theme-option{
  border:2px solid hsl(224, 36%, 15%);
  border-radius: 20px;
  /* background-color:hsl(223, 31%, 20%); */
  height:75%;
  background-color: hsl(224, 36%, 15%);
  display:flex;
  justify-content: space-around;
  align-items: center;
}
.theme-white{
  /* background-color:hsl(223, 31%, 20%); */
  /* padding :7%; */
  /* border-radius:50%; */
  cursor:pointer;
  /* border:2px solid red; */
}
.theme-white .active{
  opacity:0.5;
}
/* .theme-white:hover{
  background-color:hsl(225, 21%, 49%);

} */
.three{
  /* background-color:hsl(224, 36%, 15%); */
  /* border:2px solid red; */
  padding:7%;

}
.three:hover{
  border:2px solid hsl(225, 21%, 49%);
  background-color:hsl(225, 21%, 49%);
  border-radius:50%;  
  padding :7%;
}
.theme-red{
  /* background-color:hsl(223, 31%, 20%); */
  /* padding :7%; */
  /* border-radius:50%; */
  cursor: pointer;
}
.radius-color:hover{
  border-radius: 50%;
}
/* .theme-red:hover{
  background-color:hsl(6, 63%, 50%);
  border:2px solid hsl(225, 21%, 49%);
 
} */
.one{
  /* background-color:hsl(224, 36%, 15%); */
  padding:7%;

}
.one:hover{
  border:2px solid  hsl(6, 63%, 50%);
  background-color:hsl(6, 63%, 50%);
  border-radius:50%; 
  padding:7%;
}
.theme-yellow{
  /* background-color:hsl(223, 31%, 20%); */
  /* padding :7%; */
 
  /* border-radius:50%; */
  cursor: pointer;
}
.theme-yellow:hover{
  background-color:hsl(30, 25%, 89%);
  /* border:2px solid hsl(30, 25%, 89%); */
}
.two{
  /* background-color:hsl(224, 36%, 15%); */
  padding :7%;
}
.two:hover{
  background-color:hsl(30, 25%, 89%);
   border:2px solid hsl(30, 25%, 89%);
   border-radius:50%;
  padding :7%;
}


/* the change part  the first theme*/
#body.theme-yellow {
  background-color: hsl(0, 0%, 90%);
}
#body.theme-yellow .buttons-and-all {
  background-color: hsl(0, 5%, 81%);
}
#body.theme-yellow .result {
  background-color: hsl(0, 0%, 93%);
  color:black;
}
#body.theme-ye llow .reset{
  background-color: hsl(185, 58%, 25%);
  transform: translateY(2px);
  cursor: pointer;
  color: white;
}
#body.theme-yellow .reset:hover {
  background-color: hsl(185, 42%, 37%);
  box-shadow: 0 3px hsl(185, 58%, 25%);
  transform: translateY(2px);
  cursor: pointer;
  color: white;
}
#body.theme-yellow .equal-to{
  background-color:  hsl(25, 99%, 27%);
  transform: translateY(2px);
  cursor: pointer;
  color: white;
}
#body.theme-yellow .theme-option{
  background-color: hsl(0, 5%, 70%);
  border:2px solid hsl(0, 5%, 70%);
}
#body.theme-yellow .equal-to:hover {
  background-color: hsl(25, 98%, 40%);
  box-shadow: 0 3px hsl(25, 99%, 27%);
  transform: translateY(2px);
  cursor: pointer;
  color: white;
}
#body.theme-yellow .btn-number:hover {
  background-color: hsl(45, 7%, 89%);
  box-shadow: 0 2px hsl(35, 11%, 61%);
  transform: translateY(2px);
}
#body.theme-yellow .logo p{
  color:black;
}
#body.theme-yellow .the-text p{
  color:black;
  font-weight:600;
}
/* the third change */
#body.theme-red {
  background-color: hsl(268, 75%, 9%);
  border-radius: none;
  
}
#body.theme-red .buttons-and-all {
  background-color: hsl(268, 71%, 12%)
}
#body.theme-red .result {
  background-color: hsl(268, 71%, 12%);
  color:hsl(52, 100%, 62%);
}
#body.theme-red .reset {
  background-color: hsl(281, 89%, 26%);
  box-shadow: 0 3px hsl(285, 91%, 52%);
  color: hsl(0, 0%, 100%);
}
#body.theme-red .reset:hover{
  transform: translateY(2px);
  cursor: pointer;
}
#body.theme-red .equal-to {
  background-color: hsl(176, 100%, 44%);
  box-shadow: 0 3px hsl(177, 92%, 70%);
  color: hsl(198, 20%, 13%);
}
#body.theme-red .equal-to:hover{
  transform: translateY(2px);
  cursor: pointer;
}
#body.theme-red .btn-number{
  background-color: hsl(268, 47%, 21%);
  box-shadow: 0 2px hsl(290, 70%, 36%);
  color:hsl(52, 100%, 62%);
}
#body.theme-red .btn-number:hover{
  background-color: hsl(290, 70%, 36%);
  transform: translateY(2px);
  cursor: pointer;

}
#body.theme-red .logo p{
  color:hsl(52, 100%, 62%);
}
#body.theme-red .the-text p{
  color:hsl(52, 100%, 62%);
  font-weight:600;
}
#body.theme-red .theme-option{
  background-color: hsl(268, 71%, 12%);
  border:2px solid hsl(268, 71%, 12%);
}

/* below numbers */

.below-numbers{
  display:grid;
  grid-template-columns: 1fr 1fr;
  /* border:2px solid red; */
  margin-top:5%;
}
.the-text{
  display:flex;
  justify-content: center;
  align-items: center;

}
.the-text p{
  font-size: 12px;
  color:white;
  /* border:2px solid red; */
}
/* the logo part */
.logo{
  display:flex;
  justify-content:flex-start;
  align-items:self-end;
 
}
.logo p{
  color:hsl(0, 0%, 100%);
  font-size: 15px;
  font-weight: 700;
  letter-spacing: 0.1rem;
  /* border:2px solid green; */
}



@media screen and (max-width: 900px){
    .body{
    /* border:3px solid red; */
    width:99.5%;
    background-color: hsl(222, 26%, 31%);
    min-height:99vh;
    color: hsl(0, 0%, 100%);
    display:flex;
    justify-content: center;
    align-items: center;
    }
  .the-calculator{
    width:70%;
    margin:auto;
    padding:20px 20px;
    box-shadow:  0 4px hsla(0, 1%, 63%, 0.062);
    transform: translateY(10px);
    /* border:2px solid green; */
  }
  /* buttons part */
 
  .btn-number{
    padding:10px 10px;
    /* padding-right: 25px; */
    width:75%;
    border:2px;
    font-size: 15px;
    font-weight:700;
    font-family:Arial, Helvetica, sans-serif;
    border-radius:10px;
    /* border:2px solid green; */

  }
  .btn-number:hover{
    background-color:hsl(30, 25%, 89%);
    box-shadow:  0 2px hsl(28, 16%, 55%);
    transform: translateY(2px);
  }
  .reset-equal{
    /* border:2px solid red; */
    display:grid;
    grid-template-columns: 1fr 1fr;
    row-gap: 1rem;
    margin-top:4%;
    column-gap: 0.4rem;
    padding-left:2%;
    

  }
  .unset{
    text-align: center;
    padding:1% 1%;
    /* border:2px solid yellow; */

  }
  .reset{
    background-color:  hsl(224, 28%, 35%);
    color:white;
  }
  .reset:hover{
    background-color:  hsl(225, 21%, 55%);
    box-shadow:  0 3px hsl(224, 28%, 35%);
    transform: translateY(2px);
    cursor:pointer;
    color:white;
  }
  .equal-to{
    background-color: hsl(6, 70%, 34%);
  }
  .equal-to:hover{
    background-color: hsl(6, 63%, 60%);
    box-shadow:  0 3px hsl(6, 70%, 34%);
    transform: translateY(2px);
    cursor:pointer;
    color:white;
  }
  .end-buttons{
    padding:12px;
    width:90%;
    border-radius:10px;
    border:none;
    font-size: 15px;
    font-weight:700;
    
  }
  /* the result part */
  .the-result-part{
    /* border:2px solid green; */
    width:100%;
    border:none;
    margin:15px 0;
    
  
  }
  .result{
    /* border:2px solid green; */
    width:100%;
    background-color: hsl(224, 36%, 15%);
    font-size: 40px;
    font-weight: 600;
    padding:20px 0px;
    border-radius:7px;
    border:none;
    outline:none;
    color:hsl(0, 0%, 100%);
    text-align: right;
  }
  /* logo and themes */
  .logo-and-theme{
    /* border:2px solid red; */
  display:grid;
  grid-template-columns: 8fr 4fr;
  }
  .numbers{
    display:flex;
    justify-content: right;
    align-content: center;
  }

}
</style>
