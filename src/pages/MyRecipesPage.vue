<template>
    <div class="myRecipe">
        <b-container>
            <div class="jumbotron">
                <h1 class="title">{{ this.$root.store.username }} Personal Recipes</h1>
                <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
                {{ !$root.store.username }}

                <div v-if="personalRecipes.length > 0">
                    <b-container>
                        <b-col>
                            <b-col v-for="r in personalRecipes" :key="r.id">
                                <RecipePreview class="recipePreview" :recipe="r" />
                            </b-col>
                        </b-col>
                    </b-container>
                </div>
                <div v-else>
                    personal Recipes list is empty - No personal Recipes
                </div>

            </div>
        </b-container>
    </div>
</template>


<script>
    import RecipePreview from "../components/RecipePreview";
    export default {
        components: {
            RecipePreview
        },
        data() {
            return {
                personalRecipes: [],
            }
        },
        async created(){
            try {
                const responseData = await this.axios.get(
                    "http://localhost:3000/personalRecipes/"+this.$root.store.username,
                );
                if(responseData.status == 401)
                {
                    this.$root.store.logout();
                    this.$router.push("/login");
                }
                else
                {
                    this.personalRecipes = responseData.data;
                }
            } catch (err) {
                console.log(err);
            }
        }
    }
</script>

<style>
    .myRecipePageDiv{
        text-align: center;
        background-size: cover;
        background-image: url("../pictures/background.jpg");
    }
</style>