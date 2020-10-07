<template>
  <li class="drink">
    <div class="drink-description">
      <h2 class="title">
        №{{drink.id}}.
        <span v-if="!editProcess">{{drink.name}}</span>
        <input type="text" v-model="name" v-if="editProcess">
      </h2>
      <textarea v-model="description" v-if="editProcess"></textarea>
      <p v-if="!editProcess">
        {{drink.description}}
      </p>
      <p>
        <b>Brewers tips:</b> {{drink.brewers_tips}}
      </p>
    </div>
    <div class="drink-img-wrap">
      <img class="drink-img" :src="drink.image_url">
    </div>
    <div class="buttons">
      <button v-if="!editProcess" @click="startEdit">Edit</button>
      <button v-if="!editProcess" @click="$emit('delete-drink', drink.id)">Delete</button>
      <button v-if="editProcess" @click="saveEdit(drink.id)">Save</button>
      <button v-if="editProcess" @click="cancelEdit">Cancel</button>
    </div>
  </li>
</template>

<script>
export default {
  name: 'DrinkItem',
  props: {
    drink: Object
  },
  data () {
    return {
      name: '',
      description: '',
      editProcess: false
    }
  },
  methods: {
    startEdit: function () {
      this.editProcess = true
      this.description = this.drink.description
      this.name = this.drink.name
    },
    saveEdit: function (id) {
      this.$emit('edit-drink', [id, this.name, this.description])
      this.cancelEdit()
    },
    cancelEdit: function () {
      this.editProcess = false
      this.description = ''
      this.name = ''
    }
  }
}
</script>

<style scoped lang="scss">
  .drink {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 40px;
    padding: 0 15px;
    min-height: 200px;
    &-description {
      max-width: 500px;
      .title{
        margin-top: 0;
      }
      textarea {
        width: 100%;
        height: 100px;
      }
    }
    &-img-wrap {
      display: flex;
      justify-content: center;
      /*border: solid 1px #000;*/
      margin-bottom: 20px;
      height: 200px;
      width: 200px;
      min-width: 200px;
      .drink-img {
        height: 100%;
      }
    }
    .buttons {
      display: flex;
      justify-content: center;
      width: 100%;
      button {
        width: 100px;
        height: 30px;
        cursor: pointer;
        &:not(:last-child) {
          margin-right: 10px;
        }
      }
    }
  }
</style>
