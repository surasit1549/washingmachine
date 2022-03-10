<template >
  <br><br>
  <div style="font-size: 300%" class="animate-pulse text-center text-blue-800 font-semibold">
      LAURDROMAT COMPANY    
  </div><br><br>
  <div style="width: 800px;" class="absolute border-3 border-cyan-300 border-black bg-white px-3 py-2  rounded-md flex flex-col space-y-4 right-96 border-violetl-100 bg-gradient-to-r from-blue-300 to-white-200 shadow-lg shadow-cyan-500/50 hover:drop-shadow-2xl">
    <div style="font-size: 200%" class="text-center font-semibold text-lime-600">เครื่องซักผ้าฝาบน</div>
    <div style="font-size: 100%" class="text-center font-normal text-pink-800">รุ่น T2555VSPM ระบบ Smart Inverter</div>
    <br>
    <button class="transition ease-in-out delay-150 bg-green-500 hover:-translate-y-1 hover:scale-110 hover:bg-indigo-500 duration-300 ... px-16 py-2 rounded text-gray-50 hover:bg-green-600 mx-auto" v-if="timer == 0" @click="PushCoin()">หยอดเหรียญ</button>
    <!--
    <div class="flex text-center text-red-500 px-20" v-if="timer > 0">
      <div v-if="minute > 0">&nbsp;{{ minute }}&nbsp;นาที</div>&nbsp;
      <div>{{ second }}&nbsp;วินาที</div>
    </div>
    -->
    
    <br><br>
    <div style="font-size: 150%" class="text-center">
      <h1 class="text-green-800 animate-bounce" id="myH">!!! เครื่องซักผ้าพร้อมใช้งาน !!!</h1>
    </div><br>
    
  </div>
  
</template>

<script>
  export default {
    data(){
      return {
        timer: 0,
        min: 0,
        sec: 0,
        time_limit: 80
      }
    },
    methods: {

      PushCoin(){
        this.timer = this.time_limit;
        let countdown = setInterval(()=>{
          if( --this.timer == 0 ){
            clearInterval(countdown);
          }
        },1000);
      },
      
      LineSent(){
        var data = JSON.stringify({
          "msg": "เครื่องซักผ้ากำลังทำงาน",
          "token": "6JqhXtlmKbRPLyCDR1HFFACxUAXX37VC3H5JioHgO7U"
        });      
        
        var xhr = new XMLHttpRequest();
        xhr.withCredentials = true;
        
        xhr.addEventListener("readystatechange", function() {
          if(this.readyState === 4) {
            console.log(this.responseText);
          }
        });
        
        xhr.open("POST", "https://young-savannah-95521.herokuapp.com/linenoti");
        xhr.setRequestHeader("Content-Type", "application/json");
        xhr.send(data);
      }

    },
    computed: {
      minute(){
        return parseInt(this.timer / 60);
      },
      second(){
        return this.timer % 60;
      }
    },
    watch: {
      timer(newsec,oldsec){
        if( newsec == 59 ){
          this.LineSent();
        }
        if( newsec == 0 ){
          document.getElementById("myH").innerHTML = "!!! เครื่องซักผ้าพร้อมใช้งาน !!!";
          document.getElementById("myH").style.color = "green";
        }else{
          document.getElementById("myH").innerHTML = "!!! เครื่องซักผ้ากำลังทำงาน !!!";
          document.getElementById("myH").style.color = "red";
        }
      }
    }
  }
</script>