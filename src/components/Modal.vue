<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
            <div class="modal-header">
            <slot name="header">default header</slot>
            </div>
            
                <b-modal
                id="modal-prevent-closing"
                ref="modal"
                >
                    <form ref="form" @submit.stop.prevent="addNewRecipe">

                    <!--------------------------------------------------------- Recipe Name --------------------------------------------------------->
                        <b-form-group
                            label="Recipe name:"
                            label-for="name-input"
                            invalid-feedback="name is required"
                            >
                            <b-form-input
                            v-model="name"
                                type="text"
                                placeholder="Please enter recipe name"
                                required
                            ></b-form-input>
                        </b-form-group>

                    <!--------------------------------------------------------- Image --------------------------------------------------------->
                        <b-form-group
                            label="Recipe Image URL:"
                            label-for="image-input"
                            >
                            <b-form-input
                                v-model="url"
                                type="url"
                                placeholder="Please enter URL of image" 
                                required
                            ></b-form-input>
                        </b-form-group>

                    <!--------------------------------------------------------- Ingredients list and amounts --------------------------------------------------------->
                        <label for="exampleFormControlTextarea1">Ingredients list & Amounts:</label>
                        <textarea 
                            type="text"
                            v-model="ingredients"
                            placeholder="Please enter list of ingredients and amounts"
                            class="form-control" 
                            id="exampleFormControlTextarea1" 
                            rows="3"
                        ></textarea>
                        <br>

                    <!--------------------------------------------------------- Dishes Number --------------------------------------------------------->
                        <b-form-group
                            label="Number of dishes:"
                            label-for="DishesNumber-input"
                            >
                            <b-form-input 
                                v-model="dishes"
                                placeholder="Please enter number of dishes"
                                type="number" 
                                required
                                >
                            </b-form-input>
                            </b-form-group>

                    <!--------------------------------------------------------- Cooking Time --------------------------------------------------------->
                        <b-form-group
                            label="Cooking time:"
                            label-for="cookTime-input"
                            >
                            <b-form-input 
                                v-model="cooktime"
                                type="number"
                                placeholder="Please enter time in minutes"
                                required
                            >
                            </b-form-input>
                        </b-form-group>

                    <!--------------------------------------------------------- Instructions --------------------------------------------------------->
                        <label for="exampleFormControlTextarea1">Instructions:</label>
                        <textarea 
                            v-model="instructions"
                            type="text"
                            placeholder="Please write instructions for this recipe"
                            class="form-control" 
                            id="exampleFormControlTextarea1" 
                            rows="3"
                        ></textarea>
                        <br>

                    <!--------------------------------------------------------- Vegan/ Vegetarian/ Gluten Free --------------------------------------------------------->
                        <input class="form-check-input" type="checkbox" value="" id="Vegan">
                        <label class="form-check-label" for="Vegan">
                            Vegan
                        </label>
                        <br>
                        <input class="form-check-input" type="checkbox" value="" id="Vegetarian">
                        <label class="form-check-label" for="Vegetarian">
                            Vegetarian
                        </label>
                        <br>
                        <input class="form-check-input" type="checkbox" value="" id="Gluten Free">
                        <label class="form-check-label" for="Gluten Free">
                            Gluten Free
                        </label>

                    </form>
                </b-modal>

          <div class="modal-footer">
            <slot name="footer">
              <button
                class="modal-submit-button"
                @click="addNewRecipe()"
              >Submit</button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
    props: {
        show: Boolean
    },
    data() {
        return{
            dishes: 0,
            name:"",
            url:"",
            ingredients:"",
            user_id:"",
            cooktime:0,
            instructions:"",
            DishesNumber:0,
            Instructions:"",
            GlutenFree:"",
            RecipePicture:"",
            CookingTime:0,
            Veganism:"",
            Vegeterian:"",
            RecipeName:""
        }
    },
    methods: {
        async addNewRecipe(){
            this.$emit('close');
            if(this.dishes <= 0)
                return false;
            if(this.name === "")
                return false;
            if(this.url === "")
                return false;
            if(this.ingredients === "")
                return false;
            if(this.cooktime <= 0)
                return false;
            if(this.instructions === "")
                return false;

            const response = await this.axios.post(
                    "http://localhost:3000" + "/recipes/addPrivateRecipe",
                    
                    {
                        user_id: this.user_id,
                        dishesNumber: this.DishesNumber,
                        instructions: this.Instructions,
                        gluten_free: this.GlutenFree,
                        recipePic: this.RecipePicture,
                        cookingTime: this.CookingTime,
                        vegan: this.Veganism,
                        vegetarian: this.Vegeterian,
                        recipeName: this.RecipeName
                    }
                );
            }
        }
}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 700px;
  margin: 15px auto;
  padding: 10px 30px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  color: #42b983;
}

.modal-submit-button {
  background-color: #42b983;
  font-size: 130%;
  border: 0px ;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>