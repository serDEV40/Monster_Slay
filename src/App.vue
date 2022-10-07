<script>
  export default{
    data(){
      return{
        playerHeal : 100,
        monsterHeal: 100,
        game_is_on : false,
        log_out : [],
        friendHealth : {
          backgroundColor : "#4863A0",
          color : "#fff"
        },
        enemyHealth : {
          backgroundColor : "#FF0000",
          color : "#fff"
        }
      }
    },

    methods:{
      start_game : function(){
        this.log_out.push("Oyun Başladı");
        return(this.game_is_on = !this.game_is_on);
      },

      attack : function(){
        var point = Math.ceil(Math.random()*10);
        this.monsterAttack();
        this.monsterHeal-=point;
        this.log_out.push(("Senin Canın: "+this.playerHeal));
        this.log_out.push(("Canavarın Canı: "+this.monsterHeal));
      },

      special_attack : function(){
        var point = Math.ceil(Math.random()*40);
        this.monsterHeal-=point;
        this.monsterAttack();
        this.log_out.push(("Senin Canın: "+this.playerHeal));
        this.log_out.push(("Canavarın Canı: "+this.monsterHeal));
      },

      healUp : function(){
        var point = Math.ceil(Math.random()*20);
        this.playerHeal+=point;
        this.monsterAttack();
        this.log_out.push(("Senin Canın: "+this.playerHeal));
      },

      giveUp : function(){
        this.playerHeal = 0;
        this.log_out.push(("Senin Canın: "+this.playerHeal));
      },
      monsterAttack : function(){
        var point = Math.ceil(Math.random()*15);
        return(this.playerHeal-=point);
      }
    },

    watch : {
      playerHeal : function(e){
        if(e <= 0){
          this.playerHeal = 0;
          if(confirm("Oyunu Kaybettin Tekrar Oynamak İster Misin?")){
            this.monsterHeal = 100;
            this.playerHeal = 100;
          }
        }else if(e >= 100){
          this.playerHeal = 100;
        }
      },
      monsterHeal : function(val){
        if(val <= 0){
          this.monsterHeal = 0;
          if(confirm("Oyunu Kazandın Tekrar Oynamak İster Misin")){
            this.monsterHeal = 100;
            this.playerHeal = 100;
          }
        }else if(val >= 100){
          this.monsterHeal = 100;
        }
      }
    }
  }
</script>

<template>
   <section class="row" v-if="game_is_on">
        <div class="small-6 columns">
            <h1 class="text-center">SEN</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" :style="{width : playerHeal+'%'}">
                  {{playerHeal}}
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">CANAVAR</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" :style="{width:monsterHeal+'%'}" >
                  {{monsterHeal}}
                </div>
            </div>
        </div>
    </section>

    <section class="row controls" v-if="!game_is_on">
        <div class="small-12 columns">
            <button id="start-game" @click="start_game">YENİ OYUN</button>
        </div>
    </section>

    <section class="row controls" v-if="game_is_on">
        <div class="small-12 columns">
            <button id="attack" @click="attack">SALDIRI</button>
            <button id="special-attack" @click="special_attack">ÖZEL SALDIRI</button>
            <button id="heal" @click="healUp">İLK YARDIM</button>
            <button id="give-up" @click="giveUp">PES ET!</button>
        </div>
    </section>

    <section class="row log" v-if="game_is_on">
        <div class="small-12 columns">
            <ul v-for="(log,indexNumber) in log_out">
                <li v-if="(indexNumber % 2 == 1)" :style="friendHealth">{{log}}</li>
                <li v-else-if="(indexNumber % 2 == 0)" :style="enemyHealth">{{log}}</li>
            </ul>
        </div>
    </section>
</template>

<style>
  button{
    padding:0.5rem 1rem;
    margin:  0.5rem 1rem;
  }
</style>
