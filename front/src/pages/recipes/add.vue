<template>
    <div class="flex flex-col md:px-32 px-12 pt-12">
        <div class="flex flex-row items-center">
            <router-link to='/profile' class="rounded bg-gray-200 py-1 px-4">Back</router-link>
            <div  class="font-bold  text-3xl ml-5">
                Create recipe
            </div>
        </div>
        <form class="bg-white w-full mt-10 md:w-full shadow-md rounded px-8 pt-6 pb-8 mb-4">
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                    Title
                </label>
                <input v-model="title" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="title" type="text" placeholder="Title">
            </div>
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                    Description
                </label>
                <textarea v-model="description" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="description" type="text" placeholder="Description"/>
            </div>
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                    Ingredients
                </label>
                <div class="md:flex w-full items-center">
                    <div class="w-full pr-1">
                        <input v-model="item" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="title" type="text" placeholder="Enter one ingredient at a time...">
                    </div>
                    <div class="h-full p-1 mt-5 md:mt-0">
                        <a  @click="addIngredient" class="bg-red-500 py-2 text-white px-6 rounded">Add</a>
                    </div>
                </div>
            </div>
            <div class="my-8">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                    Ingredients
                </label>
                <div v-for="ingredient in ingredients" :key="ingredient">
                    <div class="flex flex-row my-6 justify-between">
                    <div class="font-semibold">
                        {{ingredient}}
                    </div>
                    <div @click="deleteItemIngredient(ingredient)" class="font-medium"> 
                        Delete
                    </div>
                </div>
                </div>
            </div>
            <div class="flex items-center justify-center">
                <button @click="createRecipe" class="bg-red-600 rounded w-full md:w-1/6  text-white font-bold py-2 px-4 focus:outline-none focus:shadow-outline" type="button">
                    Create
                </button>
            </div>
        </form>
    </div>
</template>
<script>
import {createRecipe} from '../../constants/recipe'
export default {
    name:'recipes-add',
    data() {
        return {
            title:null,
            description: null,
            item:null,
            ingredients:[],
            error:{
                active : false,
                message: ''
            }
        }
    },
    methods: {
        deleteItemIngredient(item){
            const backupArrayIngredients = this.ingredients
            
            const index = backupArrayIngredients.indexOf(item);
            if (index > -1) {
                backupArrayIngredients.splice(index, 1);
            }

            this.ingredients = backupArrayIngredients
        },
        addIngredient(){
            this.ingredients.push(this.item)
            this.item = ""
        },
        async createRecipe() {
            try {
                const user = JSON.parse(localStorage.getItem('user'))
                const data = await createRecipe({
                    userid: user._id,
                    title : this.title,
                    description : this.description,
                    ingredients : this.ingredients
                })

                if(data.status){
                    this.$router.push('/profile')
                }else{
                    this.error.active = true
                    this.error.message = "Something was wrong to create the recipe."
                }
            } catch (error) {
                this.error.active = true
                    this.error.message = "Something was wrong to create the recipe."
            }
        }
    },
}
</script>