<template>
    <h1 class=" text-5xl text-center m-12">Matrix Rechner!</h1>

        <div class="flex justify-around w-full ">
            <!--Matrix A-->
            <div class=" ml-6 inline-block p-10 bg-white shadow-2xl rounded-xl border-1 border-gray-400"> 
                <div class="flex flex-col items-center">
                    <h2 class=" mb-4 mt-4 text-lg">Matrix A</h2>
                    <div class="ml-1 mb-4">
                        <button v-on:click="() => {A_spalten_adder(); dim_checker()}" 
                        class=" rounded bg-gray-900 h-10 w-10 text-white">+
                        </button>
                        <button v-on:click="() => {A_spalten_remover(); dim_checker()}" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                        <button v-on:click="() => {A_zeilen_adder(); dim_checker()}" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-4">+
                        </button>
                        <button v-on:click="()=> {A_zeilen_remover(); dim_checker()}" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                    </div>
                </div>
            
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

            <!--Operationen Buttons-->
            <div class="flex flex-col">
                <!--Multiplikation-->
                <button
                    v-on:click="() => {matrix_multiplikator()}"
                    class="bg-gray-900 h-10 w-30 text-white text-center transition-colors duration-300 mt-12 mb-6"
                    :class="{ 'hover:bg-red-700': !dim_multi}"
                    >A * B
                </button>

                <!--Addition-->
                <button
                    v-on:click="() => {matrix_addierer()}"
                    class="bg-gray-900 h-10 w-30 text-center transition-colors duration-300 mb-6 text-white"
                    :class="{ 'hover:bg-red-700': !dim_addition}"
                    >A + B
                </button>

                <!--Subraktion-->
                <button
                    v-on:click="() => {matrix_subtrahierer()}"
                    class="bg-gray-900 text-white h-10 w-30 text-center transition-colors duration-300"
                    :class="{ 'hover:bg-red-700': !dim_addition}"
                    >A - B
                </button>
            </div>

            
            <!--MATRIX B-->
            <div class="inline-block p-10 bg-white shadow-2xl rounded-xl border-1 border-gray-400 mr-4"> 
                <div class="flex flex-col items-center">
                    <h2 class=" mb-4 mt-4 text-lg">Matrix B</h2>
                    <div class="flex justify-end mr-4 mb-4">
                        <button v-on:click="() => {B_spalten_adder(); dim_checker()}" 
                        class="rounded bg-gray-900 h-10 w-10 text-white">+
                        </button>
                        <button v-on:click="() => {B_spalten_remover(); dim_checker()}" 
                        class=" rounded bg-gray-900 h-10 w-10 text-white ml-1">-
                        </button>
                        <button v-on:click="() => {B_zeilen_adder(); dim_checker()}" 
                        class="rounded bg-gray-900 h-10 w-10 text-white ml-4">+
                        </button>
                        <button v-on:click="() => {B_zeilen_remover(); dim_checker()}" 
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
        
    <!--MATRIX C-->
    <div class="flex justify-center w-full mt-12">
        <table v-if="show_C">
            <tr v-for="(zeile,i) in C_matrix" :key="i">
                <td v-for="(value, j) in zeile" :key="j">
                <button class="bg-gray-300 w-12 text-center ml-2 mr-2 mb-1 border-black border-1">
                    {{ C_matrix[i][j] }}
                </button>
                </td>
            </tr>
        </table>
    </div>
   

    </template>

<script setup lang="ts">
    import { ref } from 'vue'
    const dim_multi = ref(true)
    const dim_addition = ref(true)
    const show_C = ref(false)
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
        update_C_matrix()
        if(B_anzahl_zeilen.value === A_anzahl_spalten.value){
            show_C.value = true
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
        C_matrix.value = Array.from({ length: A_anzahl_zeilen.value }, () =>
            Array.from({ length: B_anzahl_spalten.value }, () => 0)
        )
        }


    function matrix_addierer(){
        update_C_matrix()
        if(A_anzahl_spalten.value === B_anzahl_spalten.value){
            if(A_anzahl_zeilen.value === B_anzahl_zeilen.value){
                show_C.value = true
                for(let i = 0; i < A_anzahl_zeilen.value; i++){
                    for(let j = 0; j < A_anzahl_spalten.value; j++){
                        C_matrix.value[i][j] = A_matrix.value[i][j]+ B_matrix.value[i][j]; 
                    }
                }
            }
        }
    }

    function matrix_subtrahierer(){
        update_C_matrix()
        if(A_anzahl_spalten.value === B_anzahl_spalten.value){
            if(A_anzahl_zeilen.value === B_anzahl_zeilen.value){
                show_C.value = true
                for(let i = 0; i < A_anzahl_zeilen.value; i++){
                    for(let j = 0; j < A_anzahl_spalten.value; j++){
                        C_matrix.value[i][j] = A_matrix.value[i][j] - B_matrix.value[i][j]; 
                    }
                }
            }
        }
    }

    function dim_checker(){
        show_C.value = false
        if(A_anzahl_spalten.value === B_anzahl_spalten.value){
            if(A_anzahl_zeilen.value === B_anzahl_zeilen.value){
                dim_addition.value = true
        } else{dim_addition.value = false}
    } else{dim_addition.value = false}

        if(A_anzahl_spalten.value === B_anzahl_zeilen.value){
            dim_multi.value = true
        } else dim_multi.value = false
    }    
    </script>