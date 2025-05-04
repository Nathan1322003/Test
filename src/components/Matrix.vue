<template>
    <h1 class=" text-5xl text-center m-12">Matrix Rechner!</h1>

        <div class="flex justify-around w-full ">
            <div class=" ml-6 inline-block p-10 bg-white shadow-2xl rounded-xl border-1 border-gray-400"> 
                <div class="flex flex-col items-center">
                    <h2 class=" mb-4 mt-4 text-lg">Matrix A</h2>
                    <div class="ml-1 mb-4">
                        <button v-on:click="A_spalten_adder" 
                        class=" rounded bg-gray-900 h-10 w-10 text-white">+
                        </button>
                        <button v-on:click="A_spalten_remover" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                        <button v-on:click="A_zeilen_adder" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-4">+
                        </button>
                        <button v-on:click="A_zeilen_remover" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                    </div>
                </div>
                
                <!--Matrix A-->
                <table>
                    <tr v-for="(zeile,i) in A_matrix" :key="i">
                        <td v-for="(value, j) in zeile" :key="j">
                            <input type=""
                            v-model.number = A_matrix[i][j]
                            class=" bg-gray-300 w-12 text-center ml-2 mr-2 mb-1 border-black border-1">
                        </td>
                    </tr>
                </table>
            </div>

            <!--Multiplikation-->

            <button v-on:click="update_C_matrix" class=" bg-green-800 h-10 w-30 text-center">A * B
            </button>
            
            <!--MATRIX B-->
            <div class=" ml-6 inline-block p-10 bg-white shadow-2xl rounded-xl border-1 border-gray-400 mr-4"> 
                <div class="flex flex-col items-center">
                    <h2 class=" mb-4 mt-4 text-lg">Matrix B</h2>
                    <div class="flex justify-end mr-4 mb-4">
                        <button v-on:click="B_spalten_adder" 
                        class="rounded bg-gray-900 h-10 w-10 text-white">+
                        </button>
                        <button v-on:click="B_spalten_remover" 
                        class=" rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                        <button v-on:click="B_zeilen_adder" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-4">+
                        </button>
                        <button v-on:click="B_zeilen_remover" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                </div>
                </div>
                
                <table>
                    <tr v-for="(zeile,i) in B_matrix" :key="i">
                        <td v-for="(value, j) in zeile" :key="j">
                            <input type=""
                            v-model.number = B_matrix[i][j]
                            class=" bg-gray-300 w-12 text-center ml-2 mr-2 mb-1 border-black border-1">
                        </td>
                    </tr>
                </table>
            </div>
        </div>
    <p v-if="falsche_dim">ERROR</p>
   
    </template>

<script setup lang="ts">
    import { ref, watch } from 'vue'
    const falsche_dim = ref(false)
    const A_anzahl_spalten = ref(3)
    const A_anzahl_zeilen = ref(3)
    const A_matrix = ref<number[][]>(
        Array.from({ length: A_anzahl_zeilen.value }, () =>
        Array.from({ length: A_anzahl_spalten.value }, () => 0)
        ))


    const B_anzahl_spalten = ref(3)
    const B_anzahl_zeilen = ref(3)
    const B_matrix = ref<number[][]>(
        Array.from({ length: B_anzahl_zeilen.value }, () =>
        Array.from({ length: B_anzahl_spalten.value }, () => 0)
        ))

    const C_matrix = ref<number[][]>(
        Array.from({ length: A_anzahl_zeilen.value }, () =>
        Array.from({ length: B_anzahl_spalten.value }, () => 0)
        ))

    function A_spalten_adder(){
        A_anzahl_spalten.value ++
        A_matrix.value.forEach(value => value.push(0))
    }

    function A_spalten_remover(){
        if(A_anzahl_spalten.value > 1){
            A_anzahl_spalten.value --
            A_matrix.value.forEach(value => value.pop())}
        

    }

    function A_zeilen_adder(){
        A_anzahl_zeilen.value ++
        A_matrix.value.push(Array.from({ length: A_anzahl_spalten.value }, () => 0))}

    function A_zeilen_remover(){
        if (A_anzahl_zeilen.value > 1){
            A_anzahl_zeilen.value --
            A_matrix.value.pop()}
        
    }


    function B_spalten_adder(){
        B_anzahl_spalten.value ++
        B_matrix.value.forEach(value => value.push(0))
    }

    function B_spalten_remover(){
        if(B_anzahl_spalten.value > 1){
            B_anzahl_spalten.value --
            B_matrix.value.forEach(value => value.pop())}
        

    }

    function B_zeilen_adder(){
        B_anzahl_zeilen.value ++
        B_matrix.value.push(Array.from({ length: B_anzahl_spalten.value }, () => 0))}

    function B_zeilen_remover(){
        if (B_anzahl_zeilen.value > 1){
            B_anzahl_zeilen.value --
            B_matrix.value.pop()}
        
    }

    function matrix_multiplikator(){
        if(B_anzahl_zeilen.value === A_anzahl_spalten.value){
            for(let i = 0; i < A_anzahl_zeilen.value; i++){
                for(let j = 0; j < B_anzahl_spalten.value; j++){
                    let sum = 0;
                    for(let k = 0; k < A_anzahl_spalten.value; k++){
                        sum += A_matrix.value[i][k] * B_matrix.value[k][j];

                    }
                    C_matrix.value[i][j] = sum;

                }
                    }
            }
                
    }
    function update_C_matrix() {
        C_matrix.value = Array.from({ length: B_anzahl_zeilen.value }, () =>
            Array.from({ length: A_anzahl_spalten.value }, () => 0)
        )
        if(A_anzahl_spalten.value === B_anzahl_zeilen.value){
            falsche_dim.value = false
            matrix_multiplikator();
        } else { falsche_dim.value = true;

        }
        }

    // Watcher für die Dimensionen
    watch([A_anzahl_spalten, B_anzahl_zeilen], update_C_matrix, { immediate: true })
    </script>