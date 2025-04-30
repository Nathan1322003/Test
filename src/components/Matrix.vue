<template>
    <h1 class=" text-5xl text-center mt-12">Matrix Rechner!</h1>
    <button v-on:click="A_spalten_adder" class=" bg-amber-400 h-10 w-10 mt-12 ml">+
        </button>
        <button v-on:click="A_spalten_remover" class=" bg-amber-400 h-10 w-10">-
        </button>
    <div class="flex justify-around ">
        <!--MATRIX A-->

        <h2 class=" mt-32">Matrix A</h2>
        <button v-on:click="A_zeilen_adder" class=" bg-amber-800 h-10 w-10">+
        </button>
        <button v-on:click="A_zeilen_remover" class=" bg-amber-800 h-10 w-10">-
        </button>
        <div>
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
        
    

        
        <!--MATRIX B-->
        <h2 class=" mt-32">Matrix B</h2>
        <button v-on:click="B_spalten_adder" class=" bg-amber-400 h-10 w-10">+
        </button>
        <button v-on:click="B_spalten_remover" class=" bg-amber-400 h-10 w-10">-
        </button>
        <div>
            <table>
                <tr v-for="(zeile,i) in B_matrix" :key="i">
                    <td v-for="(value, j) in zeile" :key="j">
                        <input type=""
                        v-model.number = B_matrix[i][j]
                        class=" bg-red-500 w-8 rounded text-center m-1">
                    </td>
                </tr>
            </table>
        </div>
        
        <button v-on:click="B_zeilen_adder" class=" bg-amber-800 h-10 w-10">+
        </button>
        <button v-on:click="B_zeilen_remover" class=" bg-amber-800 h-10 w-10">-
        </button>
    </div>
    <button v-on:click="update_C_matrix" class=" bg-green-800 h-10 w-30 text-center">UPDATE
        </button>
    
    
    {{ C_matrix }}
    <p v-if="falsche_dim">ERROR</p>
    </template>

<script setup lang="ts">
    import { ref, watch } from 'vue'
    const falsche_dim = ref(false)
    const A_anzahl_spalten = ref(2)
    const A_anzahl_zeilen = ref(2)
    const A_matrix = ref<number[][]>(
        Array.from({ length: A_anzahl_zeilen.value }, () =>
        Array.from({ length: A_anzahl_spalten.value }, () => 0)
        ))


    const B_anzahl_spalten = ref(2)
    const B_anzahl_zeilen = ref(2)
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