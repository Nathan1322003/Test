<template>
    <div class="flex flex-col items-center">
      <!--Button 1-->
      <div class="absolute -z-10 w-2xl h-40 bg-white shadow-2xl border-1 border-gray-200 mt-10"></div>
      <h1 class="text-center text-5xl p-3 mt-12">Button 1</h1>
      <div class="flex justify-center text-3xl">
        <button v-on:click="down_1" class="bg-amber-500 rounded-lg px-7">
          -
        </button>
        <input placeholder="Eingabe..." v-model.number="number_1" class="bg-gray-500 text-white border-none p-2 rounded focus:outline-none" />
        <button v-on:click="up_1" class="bg-amber-500 rounded-lg px-7">
          +
        </button>
      </div>
  
      
      <!--Button 2-->
      <div class="absolute -z-10 w-2xl h-40 bg-white shadow-2xl border-1 border-gray-200 mt-64"></div>
      <h1 class="text-center text-5xl p-3 mt-22">Button 2</h1>
      <div class="flex justify-center text-3xl">
        <button v-on:click="down_2" class="bg-amber-500 rounded-lg px-7">
          -
        </button>
        <input placeholder="Eingabe..." v-model.number="number_2" class="bg-gray-500 text-white border-none p-2 rounded focus:outline-none" />
  
        <button v-on:click="up_2" class="bg-amber-500 rounded-lg px-7">
          +
        </button>
      </div>
  
      <!--Teiler Rückgabe-->
      <h1 class="mt-30 text-5xl">Was sind die Teiler?</h1>
      <button v-on:click="teiler" class=" mt-5 bg-amber-500 rounded-lg px-7">
        Teiler Finden
      </button>
      <h1>{{small_number}}</h1>
      <h1>{{numbers}}</h1>
      <div class="grid grid-cols-3">
        <div v-for= "i in numbers" class="h-12 w-20 bg-white m-5 text-center text-lg shadow-2xl border-1 border-gray-200"> {{i}}
        </div>
      </div>
  
    </div>
  </template>
  
  
  <script setup lang="ts">
      import { ref } from 'vue'
  
      const number_1 = ref(1)
      const number_2 = ref(1)
      const small_number = ref<number | null>(null)
      const numbers = ref<number[] | null>(null)
  
      function up_1():void{
        number_1.value = number_1.value + 1
      }
      function down_1():void {
        if(number_1.value > 0) {
          number_1.value = number_1.value - 1;
        }
       
      }
      function up_2():void {
        number_2.value = number_2.value + 1
      }
      function down_2():void {
        if(number_2.value > 0){
          number_2.value = number_2.value - 1
        }
        
      }
  
      function teiler():void {
        numbers.value = []
        if(number_1.value > number_2.value){
          small_number.value = number_2.value;
        } else{
          small_number.value = number_1.value;
        };
  
        for(let i = 1; i <= small_number.value; i++){
          if(number_1.value % i === 0){
            if(number_2.value % i === 0){numbers.value.push(i)}
          }
        }
      }
  
  
  </script>
  
  